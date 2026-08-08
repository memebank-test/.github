# memebank-test governance

Governance, shared workflow policy, and independent acceptance coordination for the `memebank-test` organization.

- Production recovery ledger: [`memebank/.github#26`](https://github.com/memebank/.github/issues/26)
- Current immutable source pins and readiness classifications: [`docs/RECOVERED_FLEET_STATUS.md`](docs/RECOVERED_FLEET_STATUS.md)
- Project and repository map: [`docs/PROJECTS.md`](docs/PROJECTS.md)
- Repository trust boundaries: [`REPOSITORY_BOUNDARIES.md`](REPOSITORY_BOUNDARIES.md)

Private production dependencies are read only from GitHub Actions through the organization-managed `TEST_FLEET_READ_TOKEN`. Never commit credentials, private media, raw OCR from real users, embeddings, clipboard contents, or production account data.
