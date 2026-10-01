# CURRENT — Brain Studio — 2026-10-01

**Status:** PUBLIC PRODUCT REPO / ARCHITECTURE LOCKED ENOUGH FOR FIRST BUILD / NOT PRODUCTION-READY

This file exists so Brain Studio continuity does not depend on one chat.

## Product boundary

Brain Studio stays focused on:
- Brain
- Skills
- Prompt Builder
- Results / Lessons
- Lab
- Smart Match
- Leaderboards

NewsStand stays KleinHub-wide and owns the broader:
- news/updates
- community
- University/lessons
- creator/community content
- challenges
- notifications/moderation

Leaderboard stays in Studio.

## Membership

Reuse one KleinHub member identity/entitlement relationship across Studio, NewsStand, University and future member products.

Do not make the currently-disabled Supabase runtime the production authority.

## Provider / AI supply

Prompt Builder is the primary provider/model selection surface.

User can:
- choose a specific provider/model
- choose Smart Match / Auto

Before run/copy show:
- prompt/input estimate
- likely output range
- selected provider/model
- why it was selected
- payment/source account
- estimated cost
- fallback cost/policy

After run show actual provider/model/token/cost truth and provenance.

Supply pool may combine:
- Brain Studio included AI
- connected BYOK providers
- official provider login/OAuth where supported
- free/included allowances
- local AI
- optional top-up balance

No silent paid fallback.

## Runtime data architecture

Supabase is not a Brain Studio runtime dependency.

Target:

```text
Prompt
→ Smart Match / proxy
→ provider
→ immediate response stream to user

async event
→ Redis Streams
→ background workers
→ Postgres product truth
→ ClickHouse high-volume telemetry
```

Use Postgres for users, membership, Brains, Skills, prompt/results/lessons and ledgers.

Use ClickHouse for tokens, cost, latency, provider/model events, quota/429, Lab evidence and Smart Match analytics.

Use Redis Streams for durable decoupling, ACK/retry/idempotency/dead-letter behavior.

## Security / money

- dedicated vault/broker for provider secrets
- envelope-encryption/Vault/Infisical pattern
- validate provider connection before activation
- pricing registry is versioned/effective-dated
- estimated cost before execution
- actual provider cost reconciled after execution
- UNKNOWN is better than fake $0
- top-ups use a ledger/hold/settle/void model
- loop/time/token/dollar budgets
- no mid-stream silent model splice after output/tool state begins

## Brain learning

Do not dump the full Brain every request.

Compile:
- core identity/rules
- compressed project facts
- only relevant Skills/files/context

Learning:

```text
Worked / Partly / Failed
→ quarantined observation
→ repeated/verified evidence
→ approval/promotion
→ Brain truth
```

One bad result must not rewrite persistent truth.

## Community

Community Skills/Prompts feed evidence, not just likes.

Leaderboards must be:
- sample-size aware
- unique-user aware
- evidence weighted
- version aware
- resistant to gaming

Guardian/Shield boundaries remain mandatory for imported community content.

## Current key docs

- `README.md`
- `docs/COMMUNITY-LEADERBOARDS.md`
- `docs/NEWSSTAND-REUSE.md`
- `docs/MEMBERSHIP-BOUNDARY.md`
- `docs/AI-SUPPLY-COST-PREVIEW.md`
- `docs/RUNTIME-DATA-ARCHITECTURE.md`

## Current important commits

- `1604e59d` — community + leaderboard direction
- `0d2e0827` — community leaderboard spec
- `e93973bb` — NewsStand reuse
- `47b71ec0` — keep Studio focused / leaderboard in Studio
- `870696ad` — Studio vs NewsStand boundary
- `9550e761` — shared membership boundary
- `f34c009e` — NewsStand as KleinHub-wide update channel
- `eb7ffcad` — public coming-soon intro
- `1d619e04` — AI supply / top-ups / cost preview
- `93f6d863` — provider/model picker + cost estimate in Prompt Builder
- `02d19768` — event-driven VPS runtime; Supabase excluded

## Next implementation target

Build the first usable vertical slice:

1. create a Brain through guided Q&A
2. add/select a Skill
3. build a portable prompt
4. choose provider/model or Smart Match
5. show expected model/source/cost
6. copy or run
7. paste/receive result
8. mark Worked / Partly / Failed
9. save lesson as quarantined evidence
10. show improved next recommendation

Do not build the entire provider marketplace/community stack before this loop works.
