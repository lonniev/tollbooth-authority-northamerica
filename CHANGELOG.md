# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
