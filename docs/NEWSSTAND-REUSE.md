# Reuse the KleinHub NewsStand for Community, Learning, and AI Updates

Brain Studio should not invent a second social/news/community shell from scratch.

The existing private repository `mightyprophets-rgb/kleinhub-ai-newsstand` already contains mature patterns that can be adapted into Brain Studio.

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

Brain Studio can use this for:
- how-to-use-AI lessons
- how to build a strong Brain
- how to make Skills
- prompting lessons
- Lab/Smart Match education
- model/provider explainers
- community creator lessons

### AI / product updates
Adapt the NewsStand article/feed model into a Brain Studio Updates section:
- new model releases
- provider changes
- new Brain Studio features
- new community Skills/Prompts
- Lab findings
- Smart Match changes
- weekly leaderboard winners
- weekly challenges
- creator spotlights
- AI safety/privacy updates

Users should be able to save updates, discuss them, and move from an update directly into a related lesson, Skill, Prompt, or Lab test.

## Brain Studio Community shape

The user-facing surface can be one area with tabs:

Community
- Trending
- Skills
- Prompts
- Leaderboards
- Challenges
- Creators

Learn
- Lessons
- Guides
- AI basics
- Brain building
- Skill building
- Prompting
- Lab / Smart Match

Updates
- Brain Studio updates
- AI/model/provider updates
- Lab findings
- Community highlights

Library
- Saved Skills
- Saved Prompts
- Saved Lessons
- Saved Updates
- History

## Important architecture rule

Reuse the NewsStand patterns and bounded components/data concepts, but do not tightly couple Brain Studio runtime to the existing NewsStand deployment.

Brain Studio owns its own product data and permissions.

## Community item evolution

NewsStand article concept maps naturally to Brain Studio content:

Article -> Update / Guide / Community Post
Save Article -> Save Skill / Prompt / Lesson / Update
Reading History -> Activity / Use History
Comments -> Community Discussion
Moderation Report -> Skill/Prompt/Comment Report
Notification -> Creator/Leaderboard/Challenge/Update notifications
University Lesson -> Brain Studio Lesson
Article Learning Links -> Skill/Prompt/Update -> related Lesson
Approval Gate -> Guardian/Shield community publication gate

## Future combined intelligence loop

AI Update
-> user reads it
-> related lesson explains it
-> related community Skills/Prompts show how to use it
-> Lab tests relevant models/prompts
-> Smart Match learns
-> Brain Studio surfaces the useful result back to members.

This preserves the KleinHub principle: easy outside, strong systems underneath.
