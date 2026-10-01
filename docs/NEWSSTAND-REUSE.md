# Reuse the KleinHub NewsStand for Community, Learning, and AI Updates

Brain Studio should not invent a second social/news/community shell from scratch.

The existing private repository `mightyprophets-rgb/kleinhub-ai-newsstand` already contains mature patterns that can be adapted into the broader Brain Studio ecosystem.

## Product boundary

### Brain Studio stays focused

Brain Studio owns the direct work surfaces:
- Brains
- Skills
- Prompt Builder
- Results / Lessons
- Lab
- Smart Match
- **Leaderboards**

The leaderboard stays in Studio because it directly helps a user choose which Skill, Prompt, creator, or approach to use next.

Studio should not become a giant social/news app.

### NewsStand becomes the broader ecosystem layer

Use the NewsStand pattern for:
- AI news and model/provider updates
- Brain Studio product updates
- community discussion
- creator/community posts
- lessons and University
- guides
- weekly challenges
- creator spotlights
- community highlights
- saved reading/library
- notifications
- moderation/reporting

This keeps Studio light while still giving Brain Studio users a rich place to learn, discover, discuss, and follow what is happening.

## Existing NewsStand capabilities worth reusing

### Community shell
- article/feed cards
- member comments and discussion
- save/bookmark
- reading history
- member library
- notifications and notification preferences
- member profiles/display names
- reporting and moderation
- admin moderation surfaces
- audit trails

Relevant existing files:
- `src/lib/member-news.ts`
- `src/components/news/member-article-tools.tsx`
- `src/routes/_authenticated/library.tsx`
- `src/lib/notifications.ts`

### Learning / University
The NewsStand already contains a structured University and lesson system:
- courses and lessons
- lesson sections
- depth layers
- practice/apply/teach-back material
- progress
- lesson governance
- lesson approval
- article-to-learning links
- published NewsStand articles usable as lesson evidence

Relevant existing areas:
- `src/lib/university/*`
- `src/components/university/*`
- `src/lib/university/lesson-sources.ts`

Use it for:
- how to use AI
- how to build a strong Brain
- how to make Skills
- prompting lessons
- Lab/Smart Match education
- model/provider explainers
- creator lessons
- privacy/safety education

### AI / product updates
Adapt the NewsStand article/feed model for:
- new model releases
- provider changes
- new Brain Studio features
- important Lab findings
- Smart Match changes
- weekly leaderboard winners
- weekly challenge announcements/results
- creator spotlights
- AI safety/privacy updates

Users should be able to save updates, discuss them, and move from an update directly into a related lesson, Skill, Prompt, Lab test, or Studio leaderboard entry.

## Navigation relationship

Brain Studio can expose lightweight links such as:

- Community
- Learn
- Updates

Those links open the NewsStand-powered ecosystem experience.

NewsStand can deep-link back into Studio:
- Add this Skill
- Try this Prompt
- Open this Brain lesson
- View leaderboard
- Test in Lab

The user should feel like one connected product family even though Studio remains operationally focused.

## Community item evolution

NewsStand article concept maps naturally to Brain Studio ecosystem content:

Article -> Update / Guide / Community Post
Save Article -> Save Update / Guide / Community Post
Reading History -> Community/learning history
Comments -> Community Discussion
Moderation Report -> Community/creator/content report
Notification -> Creator/Challenge/Update notifications
University Lesson -> Brain Studio lesson
Article Learning Links -> Update -> related Lesson / Skill / Prompt / Lab test
Approval Gate -> Guardian/Shield community publication gate

Skills and Prompts themselves remain Studio product objects even when they are surfaced/discovered through NewsStand.

## Important architecture rule

Reuse NewsStand patterns and bounded components/data concepts, but do not tightly couple Brain Studio runtime to the existing NewsStand deployment.

Brain Studio owns:
- Brain data
- Skills
- Prompts
- Results
- Lessons learned from user outcomes
- Lab evidence
- Smart Match
- Leaderboard calculations

NewsStand owns the broader community/news/learning experience.

## Future combined intelligence loop

AI Update
-> user reads it in NewsStand
-> related lesson explains it
-> related Skill/Prompt can be opened in Studio
-> Lab tests relevant models/prompts
-> Smart Match learns
-> leaderboard reflects evidence
-> NewsStand can report the useful finding back to the community.

This preserves the KleinHub principle: easy outside, strong systems underneath.
