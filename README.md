# memebank-test governance

Governance, shared workflow policy, and independent acceptance coordination for the `memebank-test` organization.

- Production recovery ledger: [`memebank/.github#26`](https://github.com/memebank/.github/issues/26)
- Current immutable source pins and readiness classifications: [`docs/RECOVERED_FLEET_STATUS.md`](docs/RECOVERED_FLEET_STATUS.md)
- Cross-organization private-source access policy: [`docs/CROSS_ORG_SOURCE_ACCESS.md`](docs/CROSS_ORG_SOURCE_ACCESS.md)
- Project and repository map: [`docs/PROJECTS.md`](docs/PROJECTS.md)
- Repository trust boundaries: [`REPOSITORY_BOUNDARIES.md`](REPOSITORY_BOUNDARIES.md)

Private production dependencies are read only from GitHub Actions through approved least-privilege credentials. `TEST_FLEET_READ_TOKEN` is the bounded transition secret name; the preferred steady state is a selected-repository GitHub App issuing short-lived installation tokens. Never commit credentials, private media, raw OCR from real users, embeddings, clipboard contents, or production account data.
