---
name: Run a TELUS Insights study-zone analysis
description: Upload a shapefile, wait for the study zones to be accepted, submit an asynchronous count job, and poll it to completion against the TELUS Insights Location API.
api: TELUS Insights Location API
source: collections/telus-insights-location-api.postman_collection.json
generated: '2026-07-25'
method: generated
operations:
  - POST /shapefile
  - GET /shapefile/{requestId}
  - GET /shapefile
  - GET /geofence
  - POST /count/{type}
  - GET /count/{type}/{jobId}
  - GET /job
  - PATCH /job/{jobid}/cancel
  - DELETE /job/{jobid}
  - GET /usage
---

# Run a TELUS Insights study-zone analysis

Every operation below is taken verbatim from the TELUS-published Postman collection saved at
`collections/telus-insights-location-api.postman_collection.json`. Base URL:
`https://location-api.insights.telus.com/product/insightsRequest/v1`.

## Before you start

- You cannot self-serve. OAuth credentials are issued by raising a ticket in the TELUS Insights
  Portal (https://insights.telus.com); the client id, client secret, token endpoint, grant type and
  scope all come from that portal's **My Account** page.
- Data starts at `2019-01-01T00:00:00`. Three days have no data at all: 2021-06-14, 2021-06-15 and
  2021-06-16. Do not treat an empty result for those dates as a bug.
- Results are aggregated (minimum 20 devices post-extrapolation), rounded up to the nearest 10, and
  extrapolated to the whole population. Never present a count as a device-level figure.

## 1. Get a token and keep it

POST the client-credentials grant to the token endpoint as
`application/x-www-form-urlencoded` with `grant_type`, `client_id`, `client_secret` and `scope`.
Read `access_token` and `expires_in` from the response.

**Reuse the token for its full lifetime — it is set to 299 seconds.** Minting a token per request is
explicitly called out as overwhelming the identity system.

Every call needs BOTH headers:

```
Authorization: Bearer {access_token}
customerId: {your customer id}
```

Omitting either returns `401 {"message":"Unauthorized"}`.

## 2. Choose your geography

You either upload your own geometry or use the pre-loaded public shapefiles.

- `GET /shapefile?selectfiles=public` lists Statistics Canada CSD and FSA boundary files that are
  already available to every account (`selectfiles` accepts `user`, `organization`, `public`, `all`).
- `GET /geofence` returns the polygons from every uploaded shapefile, with the public request IDs
  listed after the custom ones.

If you need custom geometry, `POST /shapefile` with the file as form data. Accepted formats are
`.kml`, `.zip` (ESRI: `.dbf`, `.prj`, `.shp`, `.shx`, `.cpg`) and `.json`. Constraints that will
fail the upload: file > 25 MB, more than 2000 study zones, file name > 75 characters, study-zone
name > 50 characters, a study-zone name containing a backslash or a quotation mark, a duplicate
study-zone name, or geometry that is not two-dimensional. Each polygon must carry a `study_zone`
property.

A successful upload returns `201` with a `requestId`. That id is not usable yet.

## 3. Wait for the shapefile to be accepted

`GET /shapefile/{requestId}` returns:

- `202` — still processing. Poll again.
- `200` — processed; the study zones are now queryable.
- `412` — the geometry was rejected on review ("Make sure the boundaries are well defined"). Fix and
  re-upload; the requestId is still returned so you can correlate.
- `400` — the file could not be opened at all (no requestId was created).

Study-zone names cannot be changed after upload.

## 4. Submit a count job

`POST /count/{type}` where `{type}` is one of:
`demographic`, `unique`, `origin`, `destination`, `odmatrix`, `dwelltime`, `totaltrip`,
`repeatvisitation`, `tradearea`, `home`, `work`.

Body shape (fields taken from the published examples):

```json
{
  "input_geoid":  { "requestId": "…", "study_zone": ["Kelowna", "Toronto"] },
  "output_geoid": { "requestId": "…", "study_zone": ["Calgary", "Niagara"] },
  "timezone": "UTC",
  "start_time": "2020-01-03T00:00:00",
  "end_time": "2020-01-03T01:00:00",
  "time_bucket_size": 30,
  "min_dwell_time": 15,
  "max_dwell_time": 30,
  "exclusion_types": []
}
```

Rules the API enforces or the docs require:

- `unique`, `dwelltime` and `repeatvisitation` analyse each zone independently and take
  `input_geoid` only. `home`, `work`, `origin`, `destination` and `tradearea` also need
  `output_geoid`. `odmatrix` needs at least two study zones in `input_geoid`.
- `origin` and `destination` additionally accept `pass_through_geoid`; with multiple pass-through
  zones the trip must follow the listed order exactly.
- `dwelltime` requires both `max_dwell_time` and `dwell_time_bucket`, and the bucket cannot exceed
  the maximum dwell time.
- `demographic` requires `time_bucket_size >= 1440` and takes a `topics` array.
- Time-bucket floors by range: 15 minutes up to 1 day, 60 minutes up to 30 days, 1440 minutes beyond
  30 days. The window between `min_dwell_time` and `max_dwell_time` must exceed 15 minutes.
- `exclusion_types` accepts `residents`, `non-residents`, `workers`, `non-workers`. You cannot
  combine residents with non-residents, or workers with non-workers.
- `timezone` accepts UTC, AST, CST, EST, PST, MST, NST; the default is UTC.

A valid submission returns `202` with:

```json
{ "jobId": "84f4a1ba-…", "link": { "rel": "dwelltime", "url": "/count/dwelltime/84f4a1ba-…" } }
```

`400` means the request failed validation before the job started — fix it rather than retrying.

## 5. Poll for results

`GET /count/{type}/{jobId}`.

- **Wait at least 5 minutes before the first poll**, then poll on a loop.
- `202` (or a body with `"status": "Processing"`) means keep waiting.
- `200` with `"status": "COMPLETE"` is the terminal state. Treat `status == "COMPLETE"` as the
  completion test — not the HTTP code alone.
- `404` means the job id does not exist for this customer.

Result shape: `study_zones[]`, each with a `geoid` and a `buckets[]` array, plus `bucket_size`,
`start_time` and `end_time`.

## 6. Respect the usage guidance

Rate limiting is **not** enforced, so nothing will stop you from overloading the platform. TELUS
asks for: no more than 10 queries per hour and 100 per day (averaged over full POST+GET cycles), and
no more than 5 concurrent queries including anything running in the Portal. Break large date ranges
into smaller queries to reduce processing time.

Check your own consumption with `GET /usage` (monthly call count, study zones queried, per-endpoint
breakdown).

## 7. Manage jobs

- `GET /job?limit=5&offset=0&sort=-createdDate&selectjobs=user` — paginated list, returns `206`.
  `sort` accepts `createdDate`, `api`, `status`, with a `-` prefix for descending;
  `selectjobs` accepts `user` or `organization`.
- `PATCH /job/{jobid}/cancel` — cancel a running job.
- `DELETE /job/{jobid}` — delete a job; a running job is cancelled first.

## Retry safety

**There is no idempotency key on this API.** A retried `POST /count/{type}` creates a second job and
burns a second query against the usage guidance. Store the `jobId` from the first `202` and resume
polling rather than resubmitting.
