# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
