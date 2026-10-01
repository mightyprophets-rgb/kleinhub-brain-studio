# First Worker Mission

## Goal

Build the smallest polished vertical slice of Brain Studio that proves the product loop.

## Required user journey

1. Create a Brain through a short conversational/guided Q&A.
2. Show the generated Brain in plain-language sections.
3. Add one prebuilt Skill and create one custom Skill from a natural-language request.
4. Enter a task and generate one portable copy/paste prompt from Brain + selected Skills + task.
5. Paste an external AI result back into Brain Studio.
6. Mark Worked / Partly / Failed and optionally describe what changed.
7. Write append-only Result/Success/Failure/Lesson records and reflect the lesson in the Brain.
8. Show suggested knowledge separately from confirmed/verified knowledge.

## First-build constraints

- Copy/paste first; no provider API integration required.
- No autonomous execution.
- No giant agent framework.
- Simple UI first; advanced machinery stays underneath.
- Preserve version/history semantics instead of silently overwriting past outcomes.
- Lab can be stubbed as an evidence surface in v0, but data structures must allow future model/prompt/skill tests.
- Tests must cover Brain confidence, journal append-only behavior, prompt composition, Skill generation shape, and Result -> Lesson writeback.

## Acceptance phrase

A new user can say:

> “I told it what I am working on, added what I want the AI to know how to do, copied the prompt into my AI, brought the answer back, and now it remembers what worked.”
