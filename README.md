# KleinHub Brain Studio

> **Coming soon from KleinHub**
>
> Brain Studio is being built in public as a simple way to teach AI your project, give it reusable skills, learn from what works, and carry that intelligence to whatever AI you already use.
>
> Follow the repo to watch the first worker-built version come together.

**Teach your AI once. Give it skills. Learn from every result. Use it anywhere.**

Brain Studio is a portable AI setup builder for normal people. A user creates a Project Brain through a simple Q&A, adds prebuilt or custom Skills, builds copy/paste prompts for whatever AI they already use, and records what worked or failed. The Lab learns from that evidence and improves future prompts, skills, and model recommendations.

## Product rule

Easy outside. Powerful underneath.

The first version is copy/paste-first. It does **not** require users to move their chat into Brain Studio or connect API keys.

## Core loop

Project Q&A -> Brain -> Skills -> Prompt -> Copy/Paste -> Add Result -> Worked/Partly/Failed -> Lessons -> Lab -> Better next run

## Keep Studio focused

Brain Studio itself stays intentionally light and task-focused.

Core Studio surfaces:
- **Brains**
- **Skills**
- **Prompt Builder**
- **Results / Lessons**
- **Lab**
- **Smart Match**
- **Leaderboards**

The broader social/community/news/learning experience belongs in the KleinHub NewsStand layer, not inside the main Studio workflow.

## First-version surfaces

- **Brains** — project context, goals, audience, rules, preferences, tools/context, decisions, lessons.
- **Skills** — one-click prebuilt skills plus “tell us what you want the AI to do” custom skill generation.
- **Prompt Builder** — combines Brain + chosen Skills + current task into a portable prompt with one-click copy.
- **Results** — paste an AI result back in, mark Worked / Partly / Failed, record changes and lessons.
- **Lab (member)** — tests prompts/skills/models, records evidence, and feeds findings back into the Brain. Lab may recommend; it does not silently rewrite confirmed user truth.
- **Leaderboards** — evidence-weighted weekly rankings for Skills, Prompts, creators, categories, and challenges.

## Reuse from KleinHub

Do not reinvent these concepts. Study and adapt the existing implementation in `mightyprophets-rgb/kleinhubai`:

- `docs/business-brain.md` — canonical Brain model
- `src/lib/business-brain.shared.ts` — sections, confidence, completeness
- `src/lib/brain-journal.shared.ts` — completed task / success / failure / lesson learned / recommendation / AI + human decisions
- `src/lib/dev-office/project-brief.ts` — plain-language guided Q&A composition
- `src/lib/business-coach.functions.ts` — dynamic clarifying questions when input is vague
- `src/routes/readiness.tsx` — AI-comfort/easy-vs-advanced onboarding ideas
- `src/routes/_authenticated/b.$businessId.brain.tsx` — teach-the-AI and Brain UI patterns

Reuse the concepts and bounded implementation patterns; do not couple Brain Studio to KleinHub business-office runtime.

## Brain confidence

- **Verified** — supported by result/system evidence
- **Confirmed** — explicitly provided or approved by the user
- **Suggested** — inferred by AI/Lab and awaiting review

## Initial Project Brain sections

About · Goals · Audience · Rules · Preferences · Tools & Context · Skills · Decisions · What Worked · What Didn't · Lessons · Lab Findings

## Build acceptance

A non-technical user should be able to create a Brain, add a prebuilt Skill, generate a useful prompt, copy it, paste a result back, mark whether it worked, and see the Brain/Lessons improve — without understanding prompts, agents, context windows, APIs, or orchestration.

This repository is the first real product build used to evaluate the KleinHub Pi worker system.

## Community + Leaderboards

Brain Studio users may optionally publish Skills and Prompts to the broader KleinHub community.

The **leaderboards stay inside Brain Studio** because they directly help users choose what to add or use.

Community items should show evidence, not just likes:
- uses
- Worked / Partly / Failed outcomes
- unique-user count
- Lab confidence
- strongest task categories
- strongest model matches
- version history

Leaderboards are evidence-weighted and sample-size-aware. A prompt with 5/5 successes must not automatically outrank one with 7,200/8,000 successful uses.

Initial weekly boards:
- Best Prompt This Week
- Best Skill This Week
- Fastest Rising
- Best New Creator
- Best Free-AI Skill
- Best Coding Skill
- Best Business Skill
- Community Favorite

Community results may feed the Lab only through anonymized, policy-safe evidence. Imported community Skills never gain authority to rewrite confirmed Brain rules.

Future community rewards may include featured placement, badges, free Member/Pro time, Lab credits, provider credits, sponsored challenges, prizes, or creator revenue sharing.

The flywheel is:

Community Skill/Prompt -> real usage -> Worked/Partly/Failed -> Lab evidence -> better Smart Match -> better creator feedback -> better Skills/Prompts.

## Provider + Model Intelligence

Brain Studio is the Owner-facing control surface for connected AI providers and model catalogs. It keeps provider claims, free/included/paid state, pricing/quota observations, KleinHub Lab proof, and current runtime availability visibly separate.

The ongoing architecture is documented in:

- [Provider + Model Intelligence](docs/provider-model-intelligence.md)

Core rule: provider/model catalogs are refreshed from current sources and verified again at execution time. Provider claims never replace KleinHub Lab proof, and no paid fallback is enabled silently.
