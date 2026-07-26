---
name: Book a CHR appointment
description: Authenticate with a self-signed RS512 JWT, find the patient, service, provider and location, check real availability, and create an appointment on the TELUS Health CHR Enterprise API.
api: TELUS Health CHR Enterprise API
source: graphql/telus-chr-enterprise-api.graphql
generated: '2026-07-25'
method: generated
operations:
  - patients
  - patient
  - createPatient
  - locations
  - providers
  - services
  - availabilities
  - createAppointment
  - updateAppointment
  - appointments
  - upcomingAppointments
---

# Book a CHR appointment

Every query, mutation, argument and input field named here exists verbatim in
`graphql/telus-chr-enterprise-api.graphql`, rendered from the TELUS-published introspection document
at `https://apidocs.ca.inputhealth.com/enterprise-api/introspection.json`.

## Authentication

There is no token endpoint and no client secret. You mint your own token:

- Generate a 2048-bit RSA key pair. Register the **public** key, in PEM format, on an API Consumer
  under **Settings > Enterprise API** in the CHR account.
- Sign a JWT with **RS512**.
- The `iss` claim must match the Issuer configured on that API Consumer.
- The token must expire within the next **15 minutes (900 seconds)**. Mint a fresh one per short
  batch of calls.
- Send `Authorization: Bearer {json_web_token}`.

The endpoint URL is per account: copy it from **Settings > Enterprise API > API Endpoint** in the
CHR domain you are integrating with. Develop against the sandbox domain first — production domains
hold live patient data. Every call is audit-logged and retained for 90 days.

## 1. Resolve the patient

```graphql
query FindPatient($lastName: SearchString, $dateOfBirth: ISO8601Date) {
  patients(lastName: $lastName, dateOfBirth: $dateOfBirth, first: 20) {
    edges { node { id firstName lastName dateOfBirth } }
    pageInfo { hasNextPage endCursor }
  }
}
```

`patients` also filters on `firstName`, `email`, `phone`, `gender`, `locationId`,
`primaryPractitionerId`, `statusTagId`, `identificationValue`, `identificationTemplateId`,
`includeArchived` and `orderBy`. Note that `phone` is a `PhoneSearchString` (non-numeric characters
are ignored) and the name/email fields are `SearchString` (case and surrounding whitespace ignored).

Fetch one by id with `patient(id: ID)` — it returns the patient regardless of archived status.
If the patient does not exist, create one with `createPatient(input: CreatePatientInput)`.

## 2. Resolve location, service and provider

- `locations` — takes no arguments; returns every location, each carrying its IANA time zone name.
- `services(name: SearchString, presentingIssueId: ID)`.
- `providers(lastName:, firstName:, licenseNumber:, billingNumber:, email:, languages:, hasInbox:, hasSchedule:, first:, after:)`.

## 3. Check real availability

```graphql
query Slots($from: DateTimeWithTimezone, $to: DateTimeWithTimezone, $serviceId: ID, $providers: ID, $locations: ID) {
  availabilities(from: $from, to: $to, serviceId: $serviceId, providers: $providers, locations: $locations) {
    startAt
    untilAt
  }
}
```

`availabilities` also accepts `userGroupId`, `visitType` and `exclusiveAvailability`. It respects
the account's own business rules — provider working hours, booking-time constraints — and since
version 23.22 it also accounts for **draft** appointments. Do not compute slots yourself.

`DateTimeWithTimezone` requires a `Z` or an explicit offset. Bare local datetimes will be rejected.

## 4. Create the appointment

```graphql
mutation Book($input: CreateAppointmentInput!) {
  createAppointment(input: $input) {
    id
    startAt
    untilAt
    patient { id }
    provider { id }
    location { id }
    service { id }
  }
}
```

`CreateAppointmentInput` fields: `locationId`, `patientId`, `providerId`, `respondentId`,
`serviceId`, `tagIds`, `status`, `startAt`, `untilAt`, `note`, `presentingIssueId`, `reason`,
`notifyBy`, `visitType`, `skipValidations`, `allowNotification`, `allowPractitionerNotification`.

Rules worth knowing:

- **`locationId` is required on every appointment** (mandatory since version 23.5).
- `status` at creation time is supported (added 23.24); the older `createAppointmentOptions`
  argument was removed in the same release — do not use it.
- `allowNotification` (23.25) and `allowPractitionerNotification` (26.03) control whether patient and
  practitioner notifications fire.
- `skipValidations` bypasses the account's booking rules. Treat it as privileged; leave it unset
  unless a human has explicitly authorised it for this integration.

To change a booking use `updateAppointment(input: UpdateAppointmentInput)`.

## 5. Read it back

- `appointments(from:, to:, patientId:, providerId:, locationId:, serviceId:, visitType:, includeDeactivated:, orderBy:, first:, after:)`
- `upcomingAppointments(first:, after:)`

Both are Relay connections: page with `first`/`after` (or `last`/`before`), default page size 50,
maximum 100, and follow `pageInfo.hasNextPage` / `pageInfo.endCursor`. Appointments come back
ordered by date.

## Error and retry semantics

- GraphQL field errors arrive in the `errors` array with HTTP 200 — always inspect `errors`, never
  just the status code.
- Business-rule rejections (outside working hours, conflicting booking) are errors, not empty data.
- **There is no idempotency key on this API.** A retried `createAppointment` will create a second
  appointment. Confirm with `appointments` before retrying, and prefer `updateAppointment` over
  re-creating.
- An expired token (>15 minutes) fails at the transport layer; mint a new JWT and retry.

## Staying in sync

Subscribe to the Event Notification Service topics `appointment.created`, `appointment.updated`,
`appointment.canceled` and `appointment.deactivated` rather than polling — see
`skills/telus-chr-event-notifications.md`.
