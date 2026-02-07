---
id: dashboard-page
title: Dashboard
description: Personalized home screen for authenticated users, providing an overview of their learning progress, upcoming reviews, and quick access to key features.
type: spec
subtype: page
status: draft
sequence: 003
tags: [page, dashboard, home, progress, user]
relatesTo: [progress-gamification, spaced-repetition-engine, daily-review-session, users, card_progress]
createdAt: 2026-02-07T03:34:50.682Z
updatedAt: 2026-02-07T03:34:50.682Z
---
# Dashboard Page Specification

The Dashboard page serves as the central hub for authenticated MemoryMaster users, offering an immediate overview of their learning status and quick access to essential features. It is designed to be personalized, displaying relevant statistics, upcoming tasks, and motivational elements to encourage consistent engagement. The page prioritizes clarity and actionability, allowing users to quickly resume their learning journey.

Upon logging in, users will be greeted with a welcome message, their profile information, and a summary of their daily study streak. A prominent call-to-action will highlight the number of cards due for review, enabling immediate access to their study sessions. Additionally, the dashboard will present key learning statistics, such as overall mastery levels and total cards learned, providing a visual representation of their progress over time. Quick navigation links to 'My Decks' and 'Explore Decks' will ensure seamless access to content management and discovery.

## Acceptance Criteria

*   **Personalized Welcome:** The dashboard must display a welcome message that includes the user's display name and profile picture (if available).
*   **Daily Streak & Progress:** The current daily study streak and a visual progress bar indicating daily goals must be accurately displayed.
*   **Review Session CTA:** A clear 