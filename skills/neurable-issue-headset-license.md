---
name: neurable-issue-headset-license
description: >-
  Request a signed, expiring feature license that unlocks a capability on a Neurable MW75 Neuro
  headset, keyed to the device serial number and firmware UUID. Use during device provisioning or
  activation. Critically, this endpoint reports failure with HTTP 200 and a success:false body, so
  status-code-only error handling will silently treat rejections as successes.
api: Neurable Analytics Service
base_url: https://analytics-service.neurable.com
spec: openapi/neurable-analytics-service-openapi.yml
operations:
  - create_headset_license_open_headset_license_post
generated: '2026-08-04'
method: generated
source: openapi/_original/neurable-analytics-service-openapi.json
---

# Issue a Neurable headset feature license

## Before you start

- Grounded entirely in the OpenAPI 3.1.0 document at
  `https://analytics-service.neurable.com/openapi.json`.
- This is the **only** operation on the Analytics Service tagged `open` rather than `protected`.
  That tag is the sole in-contract signal that it does not require a caller credential;
  authorization is enforced on the device serial number instead. Neurable does not document this.
- **Do not exercise this against production to explore it.** A license issued against a real serial
  number is recorded as issued — the `SERIAL_NUMBER_ALREADY_ISSUED` error exists precisely because
  the operation is not repeatable — and there is no sandbox.

## The call

`POST /open/headset/license` (`create_headset_license_open_headset_license_post`)

Body — `CreateHeadsetLicenseRequest`, all four fields required:

| Field | Type | Constraint |
|---|---|---|
| `serial_number` | string | the physical device's serial |
| `firmware_uuid` | string | the firmware build identifier on that device |
| `feature_id` | integer | **const `1`** — the spec pins this to a single value |
| `platform` | string | **const `"MW75_Neuro"`** — the only platform this operation accepts |

Note that `HardwarePlatform` elsewhere in the same spec allows `MW75_Neuro`, `IRON` and `AMP1`, but
licensing is constrained to `MW75_Neuro`. `IRON` and `AMP1` are not named anywhere on
neurable.com — treat them as internal.

## Reading the response — the important part

The `200` response is `CreateHeadsetLicenseResponse`:

```
{ "success": boolean, "detail": CreateHeadsetLicenseError | null, "license": HeadsetLicense | null }
```

**A rejection is still an HTTP 200.** Branch on `success`, never on the status code:

- `success: true` → `license` is populated, `detail` is null.
- `success: false` → `license` is null and `detail` carries one of:
  - `SERIAL_NUMBER_UNAUTHORIZED` — this serial is not eligible for the requested feature. Verify
    the device; escalate to Neurable if it should be eligible. Do not retry.
  - `SERIAL_NUMBER_ALREADY_ISSUED` — a license already exists for this serial. **Reuse the license
    you already hold**; check its `expiry_timestamp`. Do not retry.

Neither code is retryable. A retry loop on this endpoint accomplishes nothing.

## The license object

`HeadsetLicense`: `uuid`, `feature_id`, `timestamp`, `hmac`, `expiry_timestamp`.

- The `hmac` field seals the license — it is the integrity proof the device checks. Store the whole
  object verbatim; do not reconstruct or re-serialize it.
- `expiry_timestamp` means licenses are **time-limited**. Persist it and plan for re-issuance;
  nothing in the published contract describes a renewal endpoint.
- `timestamp` and `expiry_timestamp` are integers (epoch seconds); no timezone or unit is stated.

## Errors

- `422` (`HTTPValidationError`) for a malformed body — a missing field, or `feature_id`/`platform`
  outside their const values.
- All domain failures arrive as 200 + `success:false`, described above.
- See `errors/neurable-problem-types.yml`.

## Related

- `data-model/neurable-data-model.yml` — `Device`, `HeadsetLicense`, `HardwarePlatform`
- `errors/neurable-problem-types.yml` — the 200-with-error-body pattern
