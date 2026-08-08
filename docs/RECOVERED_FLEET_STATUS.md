# Recovered MemeBank fleet test status

Verified **2026-08-08**.

## Production publication

Production recovery is complete and recorded in [`memebank/.github#26`](https://github.com/memebank/.github/issues/26). Missing repositories were created, recovered histories were pushed without force, and conflict-safe `-2` repositories were retained where histories did not match.

The test organization must consume production repositories at immutable commits rather than branch names.

| Production surface | Test pin |
| --- | --- |
| `memebank/mbk-flutter` | `6beb20cdb65790d4e2890a05b777cc0a9de1efa9` |
| `memebank/mbk-desktop.rs` | `c7e5b9010bb984b6443ae00e5a4a162fc62b4cc5` |
| `memebank/mb-interfaces` | `250965b6b8aac5620419e33bef6266422a085435` |
| `memebank/mb-clients` | `e0056a1b280d2758c5d4c9d94519539c7eaece3e` |

## Test repositories

The `memebank-test` fleet separates contract, browser, mobile, storage, search, encryption, billing, and interoperability evidence:

- `rest-api-contract-e2e`
- `pwa-browser-e2e`
- `flutter-device-e2e`
- `mobile-desktop-readiness`
- `cors-origin-matrix-e2e`
- `storage-provider-interop-e2e`
- `offline-cache-sync-e2e`
- `upload-resume-e2e`
- `ocr-pipeline-e2e`
- `media-encryption-e2e`
- `embeddings-search-e2e`
- `billing-stop-e2e`
- `cliptown-image-interop-e2e`
- `clients-typescript-consumer`
- `clients-dart-consumer`

All test repositories use synthetic fixtures. Production credentials, private media, raw OCR from real users, embeddings, clipboard contents, and unredacted account data are prohibited.

## Reconciled pull requests

- `memebank-test/.github#1` was closed as superseded because current `main` already contains newer governance.
- `memebank-test/.github#2` was merged and pins the reusable SDK matrix to the recovered `memebank/mb-clients` head.
- `memebank-test/mobile-desktop-readiness#1` was merged after harness CI passed. It now verifies exact production commits, uses credential-safe private checkout, and separates ready core checks from blocked platform runners and packages.

## Credential model

Cross-organization private reads use the organization-managed GitHub Actions secret:

```text
TEST_FLEET_READ_TOKEN
```

It must be a read-only credential scoped only to the production repositories required by the test job. Workflows must never persist it in Git configuration, artifacts, logs, generated files, pull requests, or issues.

## Readiness classification

- **product regression:** an invariant fails after all declared sources, credentials, runners, and services are available;
- **blocked dependency:** a private-read credential, upstream, platform runner, package coordinate, emulator, provider sandbox, or deployment is unavailable;
- **harness regression:** a pin, fixture, script, workflow, action digest, or runner configuration is invalid.

Known blockers are retained explicitly:

- Android/iOS/macOS/Windows/Linux Flutter runner and package qualification where not yet committed;
- signed mobile and desktop distribution;
- Zed and native package coordinates;
- the final supported polyglot SDK language contract tracked in [`memebank/mb-clients#3`](https://github.com/memebank/mb-clients/issues/3).

## Change policy

When a production source head changes:

1. update the corresponding immutable pin in the owning test repository;
2. include the production PR or issue and semantic compatibility rationale;
3. run credential-free harness checks on the pull request;
4. run private live checks only from the test organization secret store;
5. attach evidence and classify failures before advancing the pin; and
6. never move a test input to an unreviewed branch tip.
