# Tollbooth Authority — North America

<p align="center">
  <img src="https://raw.githubusercontent.com/lonniev/tollbooth-dpyc/main/docs/tollbooth-hero.png" alt="Milo drives the Lightning Turnpike — Don't Pester Your Customer" width="800">
</p>

**The North American on-ramp to the Lightning turnpike.**

> Identity: `npub1zummm5awn88kw2hz6deeyjprpw0fzuv9rvzrxf3vu453yyqzvdss49xd2t`
> Parent: DPYC Prime Authority (`npub1l94pd…mq8pf`)
> Peer Authorities under Prime: [`tollbooth-authority`](https://github.com/lonniev/tollbooth-authority) (Lonnie-Authority)
> Sub-Authority: [`tollbooth-authority-newengland`](https://github.com/lonniev/tollbooth-authority-newengland)
> Service: `https://tollbooth-authority-northamerica.fastmcp.app/mcp`
> Neon region: `aws-us-west-2` (Oregon)

A peer-level Authority under DPYC Prime, serving North American operators. Operators may choose this Authority to keep certification latency and audit trails in-region.

> *The metaphors in this project are drawn with admiration from* The Phantom Tollbooth *by Norton Juster, illustrated by Jules Feiffer (1961). We just built the payment infrastructure.*

---

## Architecture

This repository is a thin consumer of the [`tollbooth-dpyc`](https://github.com/lonniev/tollbooth-dpyc) wheel's `tollbooth.authority` extension (wheel ≥ 0.22.1). The entire deployable surface lives in `src/tollbooth_authority/server.py` (~90 lines): a FastMCP instance, an OperatorRuntime, and two `register_*_tools(mcp, runtime)` calls. Onboarding state machine, Schnorr signer, replay tracker, Neon tenant provisioning, and the 16 Authority @tool definitions are wheel-resident and shared with every other Authority MCP in the ecosystem.

For the protocol semantics, certificate format (Schnorr/NIP-33 kind 30079), fee cascade, registry enforcement, and tool catalog, see the canonical [`tollbooth-authority`](https://github.com/lonniev/tollbooth-authority#mcp-tools) README — this repo is identical in behavior, distinct only in identity, region, and Neon tenant.

## Deploy

Two secrets required by Horizon:

| Env var | Value |
|---|---|
| `TOLLBOOTH_NOSTR_OPERATOR_NSEC` | This Authority's nsec |
| `NEON_DATABASE_URL` | A pooled Neon connection string in `aws-us-west-2` |

Everything else — BTCPay credentials for the cashier, operator credential templates — arrives via Secure Courier post-deploy (see [`docs/how-to-add-authority.md`](https://github.com/lonniev/dpyc-community/blob/main/docs/how-to-add-authority.md) in the community repo).

## The DPYC Ecosystem

DPYC is a federation of independent, SSE-based MCP servers sharing one Lightning-funded economy. This Authority certifies operators across the network. Sibling repos:

**Core & governance**
- [`tollbooth-dpyc`](https://github.com/lonniev/tollbooth-dpyc) — shared Python SDK (crypto, vault, auth, pricing, audit)
- [`dpyc-community`](https://github.com/lonniev/dpyc-community) — governance registry (members, rulebook, CI validation)
- [`dpyc-oracle`](https://github.com/lonniev/dpyc-oracle) — free community concierge (membership, onboarding, tax-rate questions)
- [`tollbooth-authority`](https://github.com/lonniev/tollbooth-authority) — canonical certification Authority
- [`tollbooth-sample`](https://github.com/lonniev/tollbooth-sample) — reference Operator implementation
- [`tollbooth-pricing-studio`](https://github.com/lonniev/tollbooth-pricing-studio) — native pricing editor (Swift/iOS)

**Operators (paid MCP services)**
- [`cypher-mcp`](https://github.com/lonniev/cypher-mcp) — monetized graph answers via named Cypher templates over Neo4j/AuraDB (certified under this Authority)
- [`schwab-mcp`](https://github.com/lonniev/schwab-mcp) — Charles Schwab brokerage data
- [`thebrain-mcp`](https://github.com/lonniev/thebrain-mcp) — TheBrain knowledge-graph access
- [`excalibur-mcp`](https://github.com/lonniev/excalibur-mcp) — X/Twitter posting
- [`taxsort-mcp`](https://github.com/lonniev/taxsort-mcp) — tax classification + Cloudflare Pages UI
- [`optionality-mcp`](https://github.com/lonniev/optionality-mcp) — options analytics

**Utilities & advocates**
- [`tollbooth-oauth2-collector`](https://github.com/lonniev/tollbooth-oauth2-collector) — shared OAuth2 callback collector
- [`tollbooth-shortlinks`](https://github.com/lonniev/tollbooth-shortlinks) — URL shortener

## License

Apache-2.0
