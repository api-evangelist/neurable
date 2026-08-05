---
name: neurable-download-eeg-recording
description: >-
  Download a stored EEG recording from the Neurable Analytics Service at a chosen point in the
  processing pipeline — raw, filter, feature or metric — as CSV or Parquet. Use when you hold a
  recording_id and need the signal at a specific level of processing for analysis.
api: Neurable Analytics Service
base_url: https://analytics-service.neurable.com
spec: openapi/neurable-analytics-service-openapi.yml
operations:
  - download_recording_recording_download__recording_id__get
generated: '2026-08-04'
method: generated
source: openapi/_original/neurable-analytics-service-openapi.json
---

# Download a Neurable EEG recording

## Before you start

- Grounded entirely in the OpenAPI 3.1.0 document at
  `https://analytics-service.neurable.com/openapi.json`. Neurable publishes no developer docs.
- The operation is tagged `protected` but the spec declares no `securitySchemes` — see
  `authentication/neurable-authentication.yml` and confirm the credential with Neurable.
- You need a `recording_id` (uuid). It is returned by
  `upload_recording_finalize_recording_upload_finalize__upload_token__post`. **There is no list or
  search operation** — if you have lost the id, the published API cannot help you find it.

## The call

`GET /recording/download/{recording_id}`
(`download_recording_recording_download__recording_id__get`)

| Parameter | In | Required | Values |
|---|---|---|---|
| `recording_id` | path | yes | uuid |
| `stage` | query | **yes** | `raw` \| `filter` \| `feature` \| `metric` |
| `format` | query | **yes** | `csv` \| `parquet` |

Both query parameters are required and **neither has a default**. Every download is an explicit
statement of which processing stage you want and in what file format.

## Choosing a stage

`RecordingStage` is Neurable's EEG processing pipeline expressed as data states:

- `raw` — the unprocessed signal as captured by the device.
- `filter` — filtered signal.
- `feature` — extracted features.
- `metric` — derived metrics.

Note the asymmetry: uploads accept only `raw` or `feature`, but downloads offer all four. `filter`
and `metric` are therefore **server-produced** — Neurable computes them. If you need a stage you did
not upload, it exists only because Neurable derived it.

## Response

The `200` response is declared as `application/json` with an **empty schema** (`{}`), so the body
shape is not described in the contract even though `format` selects `csv` or `parquet`. Treat the
payload as opaque bytes or a redirect and inspect the response `Content-Type` at runtime rather
than trusting the declared media type.

## Errors

- Only **422** (`HTTPValidationError`) is declared — typically a malformed `recording_id`, or a
  `stage`/`format` value outside the enum.
- Nothing tells you in-contract what happens when a recording exists but the requested stage has
  not been produced. Handle a non-2xx or empty body as "stage not available" and do not retry in a
  loop.
- See `errors/neurable-problem-types.yml`.

## Related

- `skills/neurable-upload-eeg-recording.md` — producing a `recording_id`
- `data-model/neurable-data-model.yml` — `RecordingStage`, `RecordingFileFormat`
- `conventions/neurable-conventions.yml` — data export formats and stages
