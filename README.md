# memebank-test governance

Governance, shared workflow policy, and independent acceptance coordination for the `memebank-test` organization.

- Production recovery ledger: [`memebank/.github#26`](https://github.com/memebank/.github/issues/26)
- Current immutable source pins and readiness classifications: [`docs/RECOVERED_FLEET_STATUS.md`](docs/RECOVERED_FLEET_STATUS.md)
- Cross-organization private-source access policy: [`docs/CROSS_ORG_SOURCE_ACCESS.md`](docs/CROSS_ORG_SOURCE_ACCESS.md)
- Project and repository map: [`docs/PROJECTS.md`](docs/PROJECTS.md)
- Repository trust boundaries: [`REPOSITORY_BOUNDARIES.md`](REPOSITORY_BOUNDARIES.md)

Private production dependencies are read only from GitHub Actions through approved least-privilege credentials. `TEST_FLEET_READ_TOKEN` is the bounded transition secret name; the preferred steady state is a selected-repository GitHub App issuing short-lived installation tokens. Never commit credentials, private media, raw OCR from real users, embeddings, clipboard contents, or production account data.


<!-- ore-org-baseline:begin -->
## Organization-wide defaults

This public repository is the canonical source for GitHub-supported community-health fallbacks, organization profile content, contribution guidance, public security/support policy, issue and pull-request templates, and agent-governance declarations for [`memebank-test`](https://github.com/memebank-test).

## Canonical organization links

- GitHub organization: https://github.com/memebank-test
- Public organization defaults: https://github.com/memebank-test/.github
- Canonical Linear project: https://linear.app/denman/project/githubcommemebank-test-6f5975c8c794
- Fleet tracking issue: https://github.com/ORESoftware/k8s-cluster/issues/1222

## Safety baseline

All Git conflicts must be resolved semantically with full historical, repository-wide, organization-wide, and relevant external-organization context. Automated agents are hard-denied from destructive or history-rewriting operations, including all forms of `git stash`, `git reset`, `git clean`, `git filter-repo`, force pushing, destructive deletion, data or infrastructure teardown, credential revocation, and policy bypass.

## GitHub inheritance boundary

GitHub can use supported community-health files from a public organization `.github` repository as fallbacks and can render `profile/README.md` on the organization page. `agents.md`, `AGENTS.md`, Copilot instructions, workflows, settings, rulesets, branch protections, permissions, and secrets are not automatically inherited merely because they exist here. Each repository must carry or synchronize compatible local policy and explicitly call reusable workflows where enforcement is required.

Generated managed-policy version: `2026-08-08`.
<!-- ore-org-baseline:end -->

<!-- BEGIN MANAGED REPOSITORY RELATIONSHIPS v1 -->
## Repository relationship registry

`memebank-test` declares repository roles, dependency edges, cross-organization capabilities, deployment ownership, and the git-submodule/Zed-package contract:

- [Human-readable map](architecture/REPOSITORY_RELATIONSHIPS.md)
- [Machine-readable manifest](architecture/repository-relationships.json)
- [JSON Schema](architecture/repository-relationships.schema.json)

The public registry withholds private repository names and edges.
<!-- END MANAGED REPOSITORY RELATIONSHIPS v1 -->
