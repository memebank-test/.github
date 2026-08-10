# Cross-organization source access

Tracking: Linear `DEN-2918`; GitHub `memebank-test/.github#8`.
Security incident: Linear `DEN-2808`; GitHub `memebank/memebank-e2e#6`.

## Policy

Test repositories may read private production source only through short-lived,
least-privilege credentials produced by an approved GitHub App or, during a
bounded transition, a fine-grained token restricted to exact repositories with
Contents read-only.

The current transition secret name is `TEST_FLEET_READ_TOKEN`. The name is not
proof that the credential is correctly scoped: its repository allowlist,
permissions, owner, expiry, rotation, and revocation procedure must be verified
separately. The preferred steady state is a selected-repository GitHub App that
mints short-lived installation tokens per workflow run.

Repository deploy keys are prohibited for generated test-fleet source checkout.
A deploy private key was committed in closed, unmerged
`memebank/memebank-e2e#1`; branch reset and source scanning do not replace
revocation. Do not copy key bytes into issues, Linear, logs, artifacts, workflow
files, replacement secrets, or incident documents.

## Required checkout contract

Every private-source checkout must:

- pin an immutable 40-character commit SHA;
- use a short-lived credential scoped to the selected source repositories;
- set `persist-credentials: false` for Actions checkout steps;
- disable unnecessary tags, LFS, and submodules unless reviewed;
- avoid embedding credentials in clone URLs or writing them to `.git/config`,
  the worktree, caches, artifacts, process output, or generated files;
- remove temporary askpass and credential helpers before the job ends;
- perform no branch, tag, release, secret, workflow, deploy-key, or package write;
- run source validation before any publication or deployment step; and
- classify missing or rejected access as a blocked dependency rather than a
  product regression.

The test-fleet App should have only Metadata read and Contents read. Any
additional permission requires a separate threat model and selected-repository
installation.

## Credential canary

A canary must prove all of the following:

1. checkout of every approved private source at the pinned SHA succeeds;
2. checkout of an unapproved private repository fails;
3. branch or tag creation fails;
4. workflow modification and dispatch with write intent fail;
5. Actions-secret and deploy-key enumeration fail;
6. the credential is absent from logs, artifacts, caches, process lists, and
   repository configuration;
7. repositories outside the documented allowlist cannot be read;
8. revoking the installation or secret makes the next checkout fail closed.

The canonical tracking issue is `memebank-test/.github#8`. Canary evidence
belongs in GitHub and is linked from Linear `DEN-2918`. Evidence records public
IDs, repository names, timestamps, workflow runs, and pass/fail outcomes—not
secret values.

## Credential-outage fallback

A credential outage must not trigger creation of a deploy key or use of a broad
reusable PAT. Tests may instead:

- validate a content-addressed source snapshot whose manifest records the
  upstream repository, immutable commit, and Git blob ID for every file; or
- skip the private-source integration with an explicit infrastructure-blocked
  result.

A content-addressed snapshot is evidence for the exact recorded files, not proof
that private source checkout is configured. Live checkout and snapshot
qualification are separate controls.

`memebank-test/cliptown-image-interop-e2e#2` demonstrates the fallback: workflow
run `31240623375` validated nine upstream Git blobs from
`memebank/mbk-rest-api@d4e630b19dad5e268ae59291c74f6190f18d3ec1`, scanned
reachable test history for private keys, and passed formatting, vet, race, and
headless SDK tests without a private repository credential.

## Rotation and incident response

Record the App installation ID or fine-grained credential ID, selected
repositories, owner, creation date, expiry, next rotation, and revocation
procedure in the approved secret-management system. Do not store an App private
key or generated installation token in GitHub source.

On suspected exposure:

1. revoke first;
2. identify the credential by public fingerprint or credential ID, never by
   copying secret bytes;
3. remove named refs and obsolete encrypted secrets;
4. scan active source and fetched history with a non-disclosing scanner;
5. qualify the replacement path in a `*-test` repository;
6. preserve only non-secret evidence and exact timestamps;
7. request history cleanup when policy requires it.

## GitHub Project fields

Track credential work in Project 1 with:

- **Linear:** `DEN-2918` or the specific security incident;
- **Control:** App installation / selected repository / canary / rotation /
  incident;
- **Source organization:** exact GitHub owner;
- **Test repository:** exact repository;
- **Credential class:** GitHub App / temporary fine-grained token / snapshot
  fallback;
- **Status:** blocked / configured / canary green / rotated / revoked;
- **Evidence:** exact workflow run or administrator record;
- **Expiry or next rotation:** absolute date;
- **Blocked by:** issue or administrator action.
