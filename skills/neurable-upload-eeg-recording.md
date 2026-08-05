---
name: neurable-upload-eeg-recording
description: >-
  Upload an EEG recording to the Neurable Analytics Service using its three-step chunked upload
  protocol — open a session, PUT ordered chunks of at most 10MB, then finalize to reassemble and
  verify. Use when you have a captured recording file from a Neurable device (MW75_Neuro, IRON or
  AMP1) and need it stored server-side and assigned a recording_id.
api: Neurable Analytics Service
base_url: https://analytics-service.neurable.com
spec: openapi/neurable-analytics-service-openapi.yml
operations:
  - upload_recording_start_recording_upload_start_post
  - upload_recording_chunk_recording_upload__upload_token__put
  - upload_recording_finalize_recording_upload_finalize__upload_token__post
generated: '2026-08-04'
method: generated
source: openapi/_original/neurable-analytics-service-openapi.json
---

# Upload an EEG recording to Neurable

## Before you start

- Neurable publishes **no** developer documentation. This skill is grounded entirely in the
  OpenAPI 3.1.0 document the service serves at `https://analytics-service.neurable.com/openapi.json`.
- All three operations are tagged `protected`. The spec declares **no** `securitySchemes` and no
  `security` requirement, so the exact token is not published. The only Neurable authorization
  server that exists is the OIDC issuer at `https://pipe.neurable.com` — see
  `authentication/neurable-authentication.yml`. **Confirm the credential with Neurable rather than
  guessing.**
- There is no sandbox. Do not exercise this flow against production with test data.

## Step 1 — open an upload session

`POST /recording/upload/start` (`upload_recording_start_recording_upload_start_post`)

Send **no file data** on this request. The body is `PostUploadRecordingStartRequest`:

| Field | Required | Shape |
|---|---|---|
| `device` | yes | `DeviceInfo`: `platform` (`MW75_Neuro` \| `IRON` \| `AMP1`), `platform_version` (int), `serial_number`, `firmware_version`, optional `platform_model` |
| `recording` | yes | `RecordingInfo`: `id` (uuid), `started_at` + `ended_at` (date-time), `timezone`, optional `category` (`general` \| `cognitive_snapshot` \| `rocket_game`, default `general`) |
| `stage` | yes | `"raw"` or `"feature"` — **only these two**, even though `RecordingStage` has four values |
| `participant_id` | no | nullable; from `create_participant_participant_post` |

The response is `PostUploadRecordingStartResponse` — a single opaque `upload_token`. Hold it; every
subsequent step is addressed by it.

## Step 2 — PUT each chunk

`PUT /recording/upload/{upload_token}` (`upload_recording_chunk_recording_upload__upload_token__put`)

- Path parameter: `upload_token` from step 1.
- Query parameter `chunk_idx` (integer, **required**): the **0-based** index of this chunk. The
  server reassembles by this index, so it — not request order — determines the byte layout.
- Body: `multipart/form-data` with a single binary part named `chunk`.
- **Maximum chunk size is 10MB** (10,485,760 bytes), stated in the operation description.

Upload chunks `0..n-1`. The spec does not state whether concurrent PUTs are safe, so upload
sequentially unless Neurable confirms otherwise.

## Step 3 — finalize

`POST /recording/upload/finalize/{upload_token}`
(`upload_recording_finalize_recording_upload_finalize__upload_token__post`)

Signals that all chunks are uploaded and should be reassembled and verified. Returns
`PostUploadRecordingFinalizeResponse`: `recording_id` (uuid) and `file_size_b` (integer).

**Verify `file_size_b` against the size of the file you sent.** This is the only integrity signal
the contract offers — there is no checksum field.

## Errors

- The only declared error status on all three operations is **422** with the FastAPI
  `HTTPValidationError` envelope: `{"detail":[{"loc":[...],"msg":"...","type":"..."}]}`. Read
  `detail[].loc` to find the offending field.
- **No 401, 403, 404, 409, 429 or 5xx response is declared**, on operations that are explicitly
  tagged `protected`. Expect undeclared statuses and handle them defensively; a bare
  `{"detail":"Not Found"}` 404 was observed live on this host.
- See `errors/neurable-problem-types.yml`.

## Retries and idempotency

Neurable documents **no** idempotency mechanism — there is no `Idempotency-Key` header and no
statement of replay behaviour. Do not assume retries are safe.

The one thing you can lean on is the protocol's own shape: because each chunk is addressed by an
explicit `upload_token` + `chunk_idx`, re-PUTting the same index *should* overwrite rather than
append. That is an inference from the contract, not a guarantee from Neurable. On an ambiguous
failure, prefer starting a fresh session over blind retry, and never call finalize twice.

## Related

- `conventions/neurable-conventions.yml` — the upload protocol, media types and identifiers
- `data-model/neurable-data-model.yml` — Device, Recording, Participant, UploadSession
- `skills/neurable-download-eeg-recording.md` — reading the recording back
