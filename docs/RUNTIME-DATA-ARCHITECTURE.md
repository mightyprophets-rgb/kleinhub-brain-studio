# Runtime Data Architecture

Supabase is **not** part of the Brain Studio runtime plan.

The previous KleinHub Supabase deployment was overwhelmed by usage and is currently not an acceptable dependency for Brain Studio. Brain Studio must use an event-driven, self-hosted data path that keeps high-volume AI telemetry away from product-state storage.

## Core runtime

User / Prompt Builder
-> Smart Match / LiteLLM proxy
-> provider
-> immediate streamed response to user

In parallel:

proxy event
-> Redis Streams
-> background workers
-> product database and analytics database

## Product database: Postgres

Use self-hosted Postgres on the VPS for durable product truth:

- users / identities
- membership / entitlements
- Brains
- Brain versions
- Skills
- Prompt versions
- Results
- Lessons
- creator/community metadata
- leaderboard configuration
- provider connection metadata (never raw secrets in ordinary tables)

This is low-to-medium volume relational state and belongs in Postgres.

## Analytics database: ClickHouse

Use ClickHouse for high-volume append-heavy AI telemetry:

- request events
- provider/model
- input/output/cache/reasoning tokens
- latency
- estimated and actual cost
- routing decisions
- fallback events
- quota / 429 events
- Lab measurements
- Smart Match observations
- leaderboard evidence aggregates
- usage history

Do not push every token/event write through the primary Postgres product database.

## Message broker: Redis Streams

Redis Streams buffers and decouples provider traffic from persistence.

Requirements:
- request/response streaming does not wait for analytics writes
- worker retries are bounded
- events are idempotent by event/log id
- consumers track acknowledgement
- poison events move to a dead-letter stream
- temporary DB failure does not block AI execution
- queue depth and lag are observable

## Cost/event record

Each completed AI request should emit a normalized event including:

- event_id
- user_id
- brain_id
- skill_id(s)
- prompt_version
- brain_version
- provider
- model_requested
- model_used
- route_mode
- source/account used
- input_tokens
- output_tokens
- cache_read_tokens
- cache_write_tokens
- reasoning_tokens
- estimated_cost
- actual_cost
- cost_provenance
- latency_ms
- fallback_count
- quota/429 details
- status
- timestamp

Later outcome feedback (Worked / Partly / Failed) attaches to the same event lineage.

## Secrets

Provider secrets belong in a dedicated encrypted credential vault / broker.

Never store:
- raw provider passwords
- browser sessions
- plaintext API keys in analytics rows
- secrets in ClickHouse

Product tables may store only provider connection IDs, encrypted secret references, scopes, status, and metadata.

## NewsStand membership migration

Brain Studio can reuse the existing NewsStand membership concepts and UI, but it must not depend on the currently-disabled Supabase runtime.

Shared KleinHub membership target:

one identity
-> VPS-hosted membership/entitlement truth
-> NewsStand
-> Brain Studio
-> future KleinHub member products

NewsStand's existing Supabase-backed tables should be treated as a migration source / prior implementation, not the new production authority, until the VPS replacement is proven.

## Scaling rule

Keep three classes of state separate:

1. **Execution path**
   - proxy/provider stream
   - latency-sensitive

2. **Product truth**
   - Postgres
   - transactional and relational

3. **AI telemetry/evidence**
   - ClickHouse
   - append-heavy and analytical

Redis sits between execution and persistence.

This prevents heavy AI usage from taking down the product database again.
