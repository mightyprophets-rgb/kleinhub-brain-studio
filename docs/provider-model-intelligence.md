# Brain Studio Provider + Model Intelligence

Status: product architecture lock  
Purpose: keep provider/model information current, compare provider claims with KleinHub Lab proof, and give the Owner one durable place to manage AI supply without hard-coding providers or models into the product.

## Product principle

Brain Studio is the human-facing provider/model control surface.

It should answer:

- What providers are connected?
- What models does each provider currently expose?
- Which models are free, included, paid, unavailable, stale, or changed?
- What does the provider claim each model can do?
- What has KleinHub Lab actually proven that model can do?
- Which pools may use it?
- What quota, reset, cost, health and runtime evidence exist?
- When was each fact last observed and from what source?

Brain Studio does **not** become the runtime router, Lab authority, workforce scheduler, or secret store.

## Truth layers

Keep these truths separate and visible:

### 1. Provider claim

Observed from the provider's current API/catalog/docs where possible:

- provider
- account/connection
- model id and display name
- access tier
- free / included / paid classification
- context window
- tool/function support
- reasoning support
- vision/audio/multimodal support
- provider pricing
- published rate limits
- published quota/reset information
- model lifecycle/deprecation state
- source
- observed_at
- freshness/staleness

Provider claim is **not** KleinHub capability proof.

### 2. KleinHub Lab proof

Observed from governed tests and independently verified outcomes:

- task class
- role/specialist fit
- machine/home
- transport/runtime
- provider/model
- tools
- context level
- verified pass/fail history
- capability state
- reliability
- latency
- observed cost/tokens
- last verified_at
- evidence references

Lab truth should appear beside provider claims, not overwrite them.

Example:

    Provider says: tools=yes, context=1M, coding
    Lab says: PROVEN CODE_MEDIUM on Laptop; OBSERVED on Desktop VERIFY

### 3. Runtime availability

Current execution truth remains separate:

- exact home/session
- post-session Pi inventory
- credential/account availability
- quota/readiness
- provider health
- route health
- current policy eligibility

A model can exist in Brain Studio but still be unavailable to a specific Pi session.

## Browser provider connection UX

Brain Studio should make provider onboarding feel like a normal browser setup flow, not a terminal or env-file task.

Owner flow:

    + Add Provider
      → choose provider
      → Brain Studio opens a secure modal
      → paste API key / token
      → optional account label
      → Connect
      → secret is sent directly to the secret broker/vault
      → browser never stores or re-renders the raw secret
      → background connection job starts
      → dashboard shows progress/result

The modal should support provider-specific fields when required, for example:

- API key
- base URL only when the provider requires a custom endpoint
- organization / account id when required
- optional label such as "Lead", "Nexus", "Atlas", or a provider account nickname

After Connect, Brain Studio should run the boring work in the background:

1. validate auth without spend where possible;
2. discover the live provider catalog;
3. classify FREE / INCLUDED / PAID / UNKNOWN;
4. capture model capabilities and provider claims;
5. identify quota/rate-limit/reset information when exposed;
6. compare current catalog with the previous observation;
7. present policy-allowed candidates to Lab;
8. update the appropriate AI-domain dashboard;
9. emit a clear success, warning, or failure notification.

The Owner experience should look like:

    Connect xKiro
    [ API key __________________ ]
    [ Connect ]

    Connecting…
    ✓ Authenticated
    ✓ 42 models discovered
    ✓ 18 free
    ✓ 11 tool-capable
    → 6 candidates ready for Lab

Do not expose raw secret values after submission. The Owner may replace/revoke a credential through an explicit action, but normal dashboards show only connection state and secret reference/last-updated metadata.

Browser key entry is an Owner convenience surface only. Secret custody remains behind the vault/broker boundary.

Background provider onboarding must not silently assign models to Lead, Worker, Nexus, or Atlas. It may recommend or pre-filter candidates; domain/pool policy remains explicit.

## Provider onboarding

When the Owner adds a provider:

    Add Provider
      → choose/auth provider
      → validate connection without spend where possible
      → discover live provider catalog
      → normalize provider/model facts
      → classify free/included/paid/unknown
      → show changes to Owner
      → Owner selects allowed models/pools
      → allowed candidates go to Lab
      → Lab returns proof
      → dashboards update

Never require the Owner to manually type a provider's complete model catalog when a trustworthy discovery API exists.

## Secrets

Brain Studio stores connection metadata and a credential reference, not raw provider secrets in normal product tables.

Example:

    provider_connection = xkiro/main
    credential_ref = secret://xkiro/main

Raw API keys/tokens belong behind the dedicated encrypted vault/broker boundary.

Never put raw secrets in:

- Postgres product rows
- ClickHouse telemetry
- Lab results
- dashboards
- logs
- public repo content

## Free-first workflow

Brain Studio must make the Owner's free-first policy easy to operate.

For each provider:

- show all discovered models
- filter/split FREE / INCLUDED / PAID / UNKNOWN
- allow free-only view
- allow explicit Owner selection
- send only policy-allowed models to Lab
- do not silently enable paid models
- if FREE becomes PAID, mark the economic state changed and remove it from free-policy eligibility until re-approved
- if pricing/access becomes UNKNOWN, do not pretend it is free

Example:

    Provider: xKiro

    FREE
      model-a      selected for Lead Lab
      model-b      selected for Worker Lab
      model-c      not selected

    PAID
      model-d      disabled by free-only policy

## Pool assignment

A provider/model may be eligible for more than one logical pool without merging pool authority.

Brain Studio may show Owner-controlled assignments such as:

- Lead
- Worker
- Vision
- Research
- Nexus
- Atlas
- product-local Router Box

Assignment means "candidate/allowed for this pool under policy", not "proven" and not "currently routable".

## AI domain split

Brain Studio is the central Owner-facing AI control room, but it must preserve independent AI supply domains rather than collapsing every provider/model into one global pool.

The primary domains are:

### Lead AI — Arty / Cord

Purpose:

- Owner-facing planning and advisory work
- Arty WHAT + HOW
- Cord WHO + WHERE + NEXT
- deep engineering and supervision

Brain Studio should show the Lead provider/model pool, current route, next eligible fallback, economic class, quota/readiness, and Lab evidence.

Lead auto-switch stays inside Lead supply. Lead failure does not silently borrow Worker Pi supply, Nexus 2.0 product/research supply, or Atlas AI.

### Worker AI — Pi-governed execution

Purpose:

- coding
- tools
- edits
- tests
- builds
- governed execution

Worker runtime availability comes from the exact governed Pi session's post-session provider/model inventory for the selected home.

Brain Studio may catalog and display Worker-capable providers/models, but a static Brain Studio or database assignment does **not** make a Worker route available. Current Worker eligibility is the intersection of:

    Brain Studio policy/catalog
      ∩ Lab capability proof
      ∩ current quota/readiness
      ∩ exact governed Pi post-session inventory

Worker AI may auto-switch inside Pi-governed Worker supply while preserving the same worker/job/worktree/checkpoint lineage.

### Nexus 2.0 AI — Product / Specialist / Research / Visual / Build

Purpose:

- Nexus product AI
- specialist/product tasks
- research
- visual/model intelligence
- Lab-facing provider/model supply
- Lovable AI build intelligence where configured

Nexus provider connections remain Nexus supply unless explicitly assigned to another domain. Nexus can auto-switch inside its own eligible pool without becoming Lead or Worker routing authority.

### Atlas AI — Owner advisor / architecture

Purpose:

- Owner advisory conversation
- architecture and system understanding
- continuity/context assistance
- bounded analysis of KleinHub state

Atlas keeps its own AI supply policy and may auto-switch among Atlas-eligible routes. Atlas AI does not become Lead, Worker, or Nexus execution authority.

### Shared provider, separate authority

The same provider company or model may appear in more than one domain, but that never merges authority.

Keep separate where applicable:

- account / credential reference
- quota domain
- economic policy
- readiness state
- Lab evidence
- route identity
- failover lineage
- Owner policy

A provider/model working in one domain is not automatically usable in another.

### Brain Studio dashboard intent

The Owner should be able to view the system at a glance:

    AI Control Room
      ├─ Lead AI — Arty / Cord
      ├─ Worker AI — Pi-governed
      ├─ Nexus 2.0 AI
      └─ Atlas AI

Each domain should expose:

- current provider/model
- eligible alternatives
- current health/readiness
- free / included / paid / unknown
- quota/reset/cooldown where known
- Lab capability state
- recent failure/failover reason
- actual provider/model receipts

Brain Studio centralizes visibility, provider/model intelligence, policy management, and Lab evidence. It does **not** centralize every runtime into one shared fallback pool.

## Lab handoff

The Lab should be able to pull candidate metadata from Brain Studio rather than rediscover provider catalogs itself.

Candidate packet should include:

- provider/model identity
- account/credential reference (never secret value)
- provider claims
- economic class
- context/tool/multimodal claims
- current catalog freshness
- requested pool/task classes
- runtime homes/transports available for test

Lab writes back proof/evidence only.

Brain Studio displays both sides:

| Model | Provider says | Lab says | Economic | Runtime | Eligible |
|---|---|---|---|---|---|
| model-a | coding, tools, 1M | PROVEN CODE_MEDIUM | FREE | Laptop ready | yes |
| model-b | reasoning, 262K | OBSERVED | FREE | Laptop ready | not yet |
| model-c | vision | REJECTED for coding | FREE | present | no for Worker |
| model-d | coding | untested | PAID | present | disabled |

## Continuous catalog refresh

Provider/model truth changes often. Do not treat one imported catalog as permanent truth.

Each provider adapter should support, where available:

- model discovery
- auth/account validation
- pricing discovery
- access-tier discovery
- quota/rate-limit evidence
- provider/model health
- deprecation/removal changes

Refresh on a bounded schedule and on explicit Owner refresh.

Every fact must preserve:

- source
- observed_at
- freshness/TTL where appropriate
- previous value/history when changed

Change classes to surface:

- MODEL_ADDED
- MODEL_REMOVED
- FREE_TO_PAID
- PAID_TO_FREE
- PRICE_CHANGED
- CONTEXT_CHANGED
- CAPABILITY_CLAIM_CHANGED
- ACCESS_TIER_CHANGED
- AUTH_CHANGED
- QUOTA_CHANGED
- PROVIDER_DEGRADED

Changes that affect safety/economics/routing should trigger re-evaluation, not silent continuation.

## Dashboard

Provider page:

    Provider
    connection/auth state
    catalog last refreshed
    quota/reset/health
    model counts by economic class
    current alerts/changes

Model row/detail:

    provider model id
    provider claims
    economic class / pricing
    context/tools/multimodal
    Lab capability state
    verified evidence summary
    pool assignments
    per-home runtime availability
    current quota/readiness
    last observed / last verified
    change history

Brain Studio should visibly distinguish:

- CURRENT
- STALE
- CHANGED
- UNVERIFIED
- OBSERVED
- PROVEN
- REVALIDATION_REQUIRED
- REJECTED
- UNAVAILABLE

## Runtime handoff

Brain Studio catalog truth is an input, not execution authority.

For governed Worker execution:

    Brain Studio/provider registry + policy
      ∩ Lab proof
      ∩ current quota/readiness
      ∩ exact home's post-session Pi inventory
      → eligible exact routes
      → KleinHub selects
      → Agent Host binds
      → Pi executes exact provider/model
      → actual receipt reconciles back to usage/Pulse/Brain Studio

Provider/model mismatch remains a runtime fault.

## Data placement

Use the existing Brain Studio data direction:

### Postgres

Durable lower-volume product truth:

- providers
- provider connections
- model catalog snapshots/current normalized rows
- pool assignments
- Owner policy
- model change records
- references to Lab proof
- credential references (not secrets)

### ClickHouse

Append-heavy observations:

- provider/model usage
- tokens/cache/reasoning
- latency
- costs
- failures/fallbacks
- quota/429 events
- catalog observation events
- Lab observation/evidence aggregates

### Redis Streams

Durable event flow:

- provider catalog refresh
- provider/model change events
- Lab candidate/test requests
- Lab result updates
- dashboard projection updates

## Non-negotiable laws

- Provider claim != Lab proof.
- Catalog presence != runtime availability.
- Runtime availability != capability proof.
- FREE/INCLUDED/PAID/UNKNOWN must remain distinct.
- UNKNOWN != FREE.
- Missing cost != $0.
- Provider/model catalogs are refreshed; they are not hard-coded architecture.
- No silent paid fallback.
- Secrets stay behind the vault/broker boundary.
- Lab tests policy-allowed candidates; it does not become provider-routing authority.
- Brain Studio displays and manages the system; it does not replace KleinHub runtime routing authority.
