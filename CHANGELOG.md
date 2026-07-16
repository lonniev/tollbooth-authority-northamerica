# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## 0.5.2 — 2026-07-16

### Changed — track tollbooth-dpyc 0.63.3

- Bumped the pinned SDK to 0.63.3 (npub-proof challenge DM now stamps the request time). Also cuts a release for changes accumulated since the last tag.

## [Unreleased]

## [0.5.1] — 2026-07-09
- chore: track tollbooth-dpyc **0.62.1** — security-hardening batch: invoice-owner check on credit settlement, GCM credential vault, encrypted self-provisioning ledger (which especially benefits Authorities), and no plaintext audit. Pin bump + uv.lock regen with no wire-API change here.

## [0.5.0] — 2026-06-29
- chore: track tollbooth-dpyc **0.57.0** — the SDK unifies the Secure Courier possession token under one name `dpop_token` (retiring `proof`/`proof_token`/`poison`). This regional Authority's proof handling lives entirely in the wheel's `authority/tools.py` (renamed there), so this is a pin bump + uv.lock regen with no wire-API change here. Picks up the wheel's free `patron_auth` probe and proof-vs-credential flow cross-references.

## [0.4.2] — 2026-06-22

### Changed — consume SDK 0.52.0 (vault_source/purchase_mode decoupling)

- **chore: track tollbooth-dpyc through 0.52.0.** Picks up the vault_source/purchase_mode decoupling: NorthAmerica Authority now explicitly uses `vault_source="env"` (self-provision Neon from env) and `purchase_mode="auto"` (derive direct/certified from registry chain; resolves to "direct" under Prime). No wire-API changes. The server.py comments clarify NorthAmerica's regional-Authority position.
- Previously tracked: **0.49.0 — REQUIRED for the operator bootstrap NIP-33 switchover.** Cold switchover with no fallback; after deploy, re-run `get_operator_config`/`register_operator` per operator. (Also carried: deferred-courtship adoption, the 0.45.3 refund-on-raise fix, the 0.47.0 dunning.)
- docs: add a DPYC ecosystem section to the README listing all federation repos, including the newcomer `cypher-mcp` (monetized graph answers) certified under this Authority.

## [0.4.1] — 2026-06-11
- chore: track tollbooth-dpyc through 0.44.15 — SDK audit hardening (correctness fixes for credit-tranche expiration in 0.44.9 and proof-reply handling in 0.44.10; blocking mypy + coverage gates). No wire-API changes.

## [0.4.0] — 2026-05-16

### Changed — collapse to thin wheel consumer (mirrors canonical 0.9.0)

Adopts the `tollbooth.authority` mixin from tollbooth-dpyc 0.22.0.
Identical refactor to canonical tollbooth-authority @ 80e7c35:

- `server.py` shrinks from ~1000 lines to ~80 (actor-specific config only)
- 8 modules deleted (actor, config, nostr_signing, onboarding, registry,
  replay, role_migration, tenant_provisioner) — code lives in
  `tollbooth.authority.*`
- `AuthorityActor` re-export from package `__init__.py` removed (no
  external consumers)

The Authority's identity (npub via env), display name, instructions,
and Neon region stay actor-specific. Everything else is the wheel's
problem now.

Pin bumped to `tollbooth-dpyc[nostr]==0.22.0`.


## [0.3.0] — 2026-05-16

### Changed — escalate onboarding to registered parent (mirrors canonical 0.8.0)

`confirm_authority_claim` / `check_authority_approval` no longer
hardcode Prime. Now use the wheel's `resolve_my_parent_npub` which
walks dpyc-community: for NorthAmerica the parent IS Prime so behavior
is unchanged, but the protocol is now generalized so sub-Authorities
like NewEngland can escalate to NA instead.

Pin bumped to `tollbooth-dpyc[nostr]==0.20.0`. Local
`_resolve_prime_npub` deleted. `OnboardingChallenge.prime_npub` →
`parent_npub`.

## [0.2.0] — 2026-05-16

### Changed — adopt tollbooth-dpyc v0.19.0, drop local proof helper

Same mirror as canonical tollbooth-authority @ 2912147: the wheel now
owns proof verification via `tollbooth.identity_proof.require_proof`.

- Pinned `tollbooth-dpyc[nostr]==0.19.0`.
- Deleted the local `_verify_operator_proof` helper (5 callers updated
  to use `require_proof` from the wheel).
- Deleted the `check_balance` override (wheel's standard now does what
  the override did).
- Deleted the `mcp._tool_manager._tools.pop(...)` workaround.

## [0.1.0] — 2026-05-16

- Initial scaffold from the `tollbooth-authority` template
- Identity: `npub1zummm5awn88kw2hz6deeyjprpw0fzuv9rvzrxf3vu453yyqzvdss49xd2t`
- Role: regional certifier for North America
- Upstream: DPYC Prime Authority
- Code identical to `tollbooth-authority`; only service-name, identity,
  and registry metadata differ
