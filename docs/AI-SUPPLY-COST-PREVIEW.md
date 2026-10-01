# AI Supply, Connected Accounts, Top-Ups, and Cost Preview

Brain Studio should make AI supply visible and understandable instead of hiding routing/cost behind an "Auto" button.

## Core idea

The more approved AI sources a member connects, the more capacity Brain Studio can use.

A member may have:

- Brain Studio included AI
- direct provider API keys
- provider-supported OAuth/device-login connections
- free/included provider allowances
- local models
- optional Brain Studio top-up credits

Brain Studio combines those into one governed supply pool.

Never ask for or store a user's raw provider password or browser session. Login-based access must use provider-supported authorization flows when available.

## Smart Match is task-aware, not a generic Auto button

Before a task runs, Brain Studio analyzes:

- task type
- selected Brain
- selected Skills
- required capabilities
- Lab evidence
- provider/model availability
- remaining quota
- speed
- privacy requirements
- user preference
- expected cost

Then it proposes the route.

Example:

Task: Debug this React component

Planned route:
1. Qwen Coder — connected provider / included
2. Claude Sonnet — fallback if needed
3. Brain Studio credits — emergency fallback only if user allows

Why:
- coding/debugging task
- Debugging Skill selected
- strongest recent Lab result for this Brain/task class
- current quota available

## Cost preview before run

The Prompt Builder / task screen should show a compact "AI Plan" before execution:

AI Plan

Primary model: Qwen Coder
Source: Your connected account
Estimated input: 8K–12K tokens
Estimated output: 2K–5K tokens
Estimated cost: $0.00 from Brain Studio
Your provider usage: applies

Fallback: Claude Sonnet
Estimated fallback cost: $0.03–$0.08
Use fallback only if primary fails: ON

[ Run ]
[ Change AI Plan ]

If cost cannot be estimated honestly, show UNKNOWN rather than fake $0.

## Cost truth after run

After execution, show:

- actual provider
- actual model
- actual input/output/cache/reasoning tokens when available
- actual provider-reported cost when available
- derived estimate only when clearly labeled
- which balance/account paid for it
- fallback events
- quota/429 events

Cost provenance:
- PROVIDER_REPORTED
- MODEL_RATE_DERIVED
- FREE_PROVEN
- CONNECTED_ACCOUNT_USAGE
- BRAIN_STUDIO_CREDIT
- UNKNOWN

## Connected AI sources

The Connections surface can show:

OpenAI       Connected
Anthropic    Connected
Gemini       Connected
xAI          Not connected
Mistral      Connected
xKiro        Connected
Requesty     Connected
Local        Available

Each connection should expose only useful truth:
- connection status
- available models
- current known quota/allowance when provider exposes it
- recent usage
- errors/429s
- last health check
- revoke/disconnect

## Usage principle

Connected provider account:
- user pays that provider under their own account/plan
- Brain Studio tracks usage/cost where technically available
- Brain Studio does not mark that usage as free merely because Brain Studio did not pay

Brain Studio included supply:
- Brain Studio pays
- hard monthly/member limits
- governed by budget ceiling

Local/free proven route:
- $0 only when zero-cost economics are genuinely proven

## Top-up packages

Members can optionally buy Brain Studio AI credits for work that exceeds included supply or when their own providers are unavailable.

Top-up credits should work across the approved Brain Studio provider pool rather than being locked to one model.

Example initial product shape (amounts/pricing TBD after real cost testing):

Small Top-Up
- light extra work

Work Pack
- larger task/project allowance

Power Pack
- heavy Lab / multi-model work

The user buys one Brain Studio credit balance; Smart Match spends from it only when policy allows.

## Spend policy

User chooses a preference:

- Use my connected AI first
- Use included/free first
- Best result within my budget
- Lowest cost
- Fastest
- Private/local first

And a hard ceiling:

Max cost for this task: $____

Brain Studio must fail closed at the ceiling.

No silent paid fallback.

## Prompt Builder integration

The Prompt Builder should not only generate the prompt. It should also show the recommended execution plan:

Prompt
Skills used
Brain context included
Task class
Recommended model
Fallback model(s)
Expected token range
Expected cost range
Who pays
Why Smart Match selected it

This turns the Prompt Builder into a transparent AI work planner while keeping the user experience simple.

## Membership effect

Free:
- basic included allowance
- limited connected providers
- manual model choice where appropriate
- basic cost preview

Member:
- more provider connections
- Smart Match
- larger included allowance
- Lab allowance
- richer usage/cost history
- top-up support

Pro:
- advanced routing policies
- larger Lab limits
- Provider Hub
- API access
- team/project budgets
- advanced analytics

Exact limits/prices must be set from real usage data, not guessed in code.

## Safety / billing rules

- Never store provider passwords.
- Never hide paid fallback.
- Never label unknown cost as zero.
- Never let NaN/null pricing bypass a budget ceiling.
- Never silently switch away from a manually pinned model.
- Always preserve provider/model/cost lineage for Lab evidence.
- Users must be able to disconnect/revoke a provider connection.


## Prompt Builder is the primary AI-selection surface

The provider/model choice belongs directly in the Prompt Builder.

The normal user flow is:

Task/Prompt
-> choose provider/model OR choose Smart Match
-> Brain Studio analyzes the actual prompt
-> estimate prompt/input size
-> estimate a reasonable output range
-> check connected/available AI supply
-> calculate a cost range from current known model pricing
-> show the planned model/source/cost before execution or copy

### Manual provider/model mode

Example:

AI
[ Claude Sonnet v ]

Prompt input estimate: ~6,800 tokens
Expected output: 2,000-4,000 tokens
Estimated API cost: $0.03-$0.05
Payment source: Your Anthropic account

The user may change provider/model and immediately see the estimate recalculate.

### Smart Match mode

Example:

AI
[ Smart Match ]

Primary:
Qwen Coder
Reason: coding/debugging task + strongest current Lab evidence + available quota
Estimated cost: uses connected/free allowance

Fallback:
Claude Sonnet
Only if primary fails
Estimated additional cost: $0.03-$0.05

The Smart Match result is based on the actual prompt/task, selected Brain, selected Skills, available connected providers/models, Lab evidence, quota, preferences, and budget.

### Copy/paste mode still gets cost guidance

Brain Studio remains useful even when it does not execute the request.

If the user selects Copy Prompt, show:

Recommended AI: Qwen Coder
Estimated prompt size: ~6,800 tokens
Estimated output range: 2,000-4,000 tokens
Estimated API cost if run through a connected/direct provider: $0.004-$0.01

[ Copy Prompt ]

This lets users decide where to paste the prompt while still understanding likely model fit and cost.

### Estimation honesty

Cost preview is an estimate until a provider returns actual usage.

The UI must distinguish:
- ESTIMATED_FROM_PROMPT
- PROVIDER_REPORTED_ACTUAL
- CONNECTED_ACCOUNT_USAGE
- FREE_PROVEN
- UNKNOWN

Prompt-token estimation and expected output ranges must never be presented as exact actual usage.

Provider/model prices used for estimates must come from a maintained pricing registry with an effective date/version so stale prices can be detected rather than silently treated as current.
