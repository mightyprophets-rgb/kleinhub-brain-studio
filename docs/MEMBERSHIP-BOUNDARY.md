# Membership Boundary

Brain Studio must not create a second independent membership/account system if the existing KleinHub NewsStand membership stack can be reused.

## Existing ecosystem

The KleinHub NewsStand already has mature member-oriented infrastructure, including:
- authenticated member accounts
- membership status/roles
- plan/entitlement fields
- member library
- comments/discussion
- notifications and preferences
- moderation/reporting
- AI usage tracking
- member/admin surfaces
- University/lesson access patterns

Brain Studio should integrate with that ecosystem rather than duplicating it.

## Rule

One user identity.
One membership relationship.
Multiple KleinHub product experiences.

NewsStand is the broader member/community/learning surface.
Brain Studio is the focused Brain/Skills/Prompt/Lab workspace.

## Suggested entitlement shape

Free:
- limited Brains
- core Brain Q&A
- basic Skills
- basic Prompt Builder
- copy/paste workflow
- limited result history
- public NewsStand/community reading where allowed

Member:
- multiple Brains
- custom Skill Builder
- full Results/Lessons history
- Lab allowance
- Smart Match
- community publishing
- saved Skills/Prompts
- creator profile
- member NewsStand/community/University features

Pro later:
- larger Lab allowance
- advanced Smart Match policies
- Provider Hub / BYOK
- API access
- advanced analytics
- teams/shared Brains
- larger usage limits

## Architecture

Brain Studio should check a shared entitlement/member contract rather than own billing truth.

Do not:
- create duplicate customer identities
- create a second conflicting subscription state
- duplicate membership status logic
- make NewsStand and Studio disagree about whether a person is a member

Studio-specific limits can be separate feature entitlements, but membership truth remains shared.

## Product benefit

A user can become a KleinHub member once and receive:
- NewsStand community + learning
- Brain Studio member features
- future connected KleinHub member products

This keeps Brain Studio focused while making the existing near-production NewsStand infrastructure immediately valuable.
