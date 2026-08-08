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

- `memebank-test/.github#1` was closed as superseded because current `main` already contained newer governance.
- `memebank-test/.github#2` was merged and pins the reusable SDK matrix to the recovered `memebank/mb-clients` head.
- `memebank-test/mobile-desktop-readiness#1` was merged after Harness CI passed. It verifies exact production commits, uses credential-safe private checkout, and separates ready core checks from blocked platform runners and packages.
- `memebank-test/.github#9` reconciled the production pins, specialized test-repository inventory, canonical transition secret name, and failure classifications.
- `memebank-test/.github#10` and `#11` established the least-privilege cross-organization source policy, prohibited generated deploy keys, preferred short-lived selected-repository GitHub App tokens, and expanded negative-permission canaries.
- `memebank-test/mobile-desktop-readiness#8` updated PyYAML after green Harness CI.
- `memebank-test/mobile-desktop-readiness#9` consolidated current Python and GitHub Action upgrades on current `main`; Harness CI run `31241201694` passed before merge.
- Stale Dependabot PRs `mobile-desktop-readiness#2` through `#7` were closed after their useful changes were preserved in `#9`.

No open pull requests remain in `memebank` or `memebank-test` as of this verification.

## Credential model

The bounded transition workflow secret name is:

```text
TEST_FLEET_READ_TOKEN
```

The name alone does not prove least privilege. It must be read-only, short-lived where possible, scoped only to the exact production repositories required by the test job, and qualified through positive and negative canaries. The preferred steady state is a selected-repository GitHub App that issues short-lived installation tokens per workflow run.

Workflows must never persist credentials in Git configuration, clone URLs, artifacts, logs, caches, process output, generated files, pull requests, or issues. Repository deploy keys are prohibited for generated source checkout. The full policy is [`CROSS_ORG_SOURCE_ACCESS.md`](CROSS_ORG_SOURCE_ACCESS.md).

## Readiness evidence

Harness CI run `31240774754` passed on `mobile-desktop-readiness#1` at exact head `eff29da4e8d829bf57f0ccd048dfc84e12dd6174`. Artifact `9016933388` has digest:

```text
sha256:9daaf04694bea5c4899e18f2bb6c7efbb7a307d0a66d52539c2b63eb3c2242e9
```

The consolidated dependency/action refresh passed Harness CI run `31241201694` at exact head `fef448c0b6931f0a18e065ebf2aa97d1272f6891` before merge.

The credential-outage fallback is also green: `memebank-test/cliptown-image-interop-e2e` workflow run `31240623375` passed scanner self-tests, reachable-history scanning, immutable Git-blob snapshot validation, containment checks, Go formatting/vet/race, and headless SDK qualification. This fallback does not prove that live private checkout is configured.

## Readiness classification

- **product regression:** an invariant fails after all declared sources, credentials, runners, and services are available;
- **blocked dependency:** a private-read credential, upstream, platform runner, package coordinate, emulator, provider sandbox, or deployment is unavailable;
- **harness regression:** a pin, fixture, script, workflow, action digest, or runner configuration is invalid.

Known blockers are retained explicitly:

- positive and negative live-checkout canary evidence under [`memebank-test/.github#8`](https://github.com/memebank-test/.github/issues/8) and Linear `DEN-2918`;
- Android/iOS/macOS/Windows/Linux Flutter runner and package qualification where not yet committed;
- signed mobile and desktop distribution;
- Zed and native package coordinates;
- the final supported polyglot SDK language contract tracked in [`memebank/mb-clients#3`](https://github.com/memebank/mb-clients/issues/3).

Scheduled specialized integration workflows may skip while a required private-source credential, platform runner, package, emulator, or other declared dependency is unavailable. Such a skip is a blocked dependency, not a passing product test.

## Change policy

When a production source head changes:

1. update the corresponding immutable pin in the owning test repository;
2. include the production PR or issue and semantic compatibility rationale;
3. run credential-free harness checks on the pull request;
4. run private live checks only through approved test-organization secret or App-token delivery;
5. attach evidence and classify failures before advancing the pin;
6. prove the credential cannot read unrelated repositories or perform writes; and
7. never move a test input to an unreviewed branch tip.
