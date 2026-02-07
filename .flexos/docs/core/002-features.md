---
id: features-mvp
title: Feature Inventory & MVP Scope
description: Details the complete feature set for MemoryMaster, organized by priority, and defines the precise scope for the Minimum Viable Product (MVP).
type: doc
subtype: core
status: draft
sequence: 2
tags:
  - features
  - mvp
  - roadmap
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## Feature Inventory

This inventory categorizes all planned features into priorities to guide development and roadmap planning. P0 features are essential for the initial launch (MVP).

### P0: MVP Essentials

These features form the core, usable product that solves the primary user problem.

*   **`user-authentication` (Secure User Authentication):** The foundation for a personalized experience. MVP will include email/password and Google social login to reduce signup friction. Apple ID login will be a fast follow.
*   **`core-flashcard-system` (Core Flashcard & Deck Management):** The fundamental content creation tool. Users must be able to create, edit, and delete decks and cards. The MVP will support text-only content on front/back fields. The database schema, however, will be designed to accommodate rich media from day one.
*   **`spaced-repetition-engine` (Intelligent Spaced Repetition):** The unique value proposition. We will implement a modified SM-2 algorithm. The review session UI must include the three self-assessment buttons ('Hard', 'Good', 'Easy') which directly feed the algorithm.
*   **`audio-pronunciation-support` (Integrated Audio Pronunciation):** A key differentiator. The MVP must include automated text-to-speech (TTS) for both front and back card fields, leveraging the Google Cloud TTS API. Custom audio uploads are a P1 feature.

### P1: High-Priority Enhancements

These features will be developed immediately post-MVP to drive retention and community growth.

*   **`progress-gamification` (Progress Tracking & Gamification):** Essential for habit formation. This includes the daily streak counter, and a calculated 'mastery level' percentage displayed per deck. These will be visualized on the user dashboard.
*   **`deck-sharing-community` (Deck Sharing & Community Hub):** We will begin with basic sharing: users can toggle a deck's visibility to 'Public' and get a shareable link. A full-featured 'Explore' page with search and filtering will follow.
*   **`user-settings-profile` (User Profile & Settings):** Allows users to manage their account (update name, change password) and set a target learning language.
*   **Rich Media Cards:** The ability to upload images to flashcards via the `card-editor-page`.

### P2: Future Roadmap

These features will expand the platform's capabilities and address a wider range of user needs.

*   Advanced Deck Discovery (ratings, comments, author profiles).
*   Offline Mode for review sessions.
*   Custom review settings (e.g., max new cards per day).
*   Collaborative Decks (allowing multiple users to contribute to a single deck).
*   Detailed learning analytics and progress reports.

## MVP Scope Definition

The MemoryMaster MVP is a focused tool that perfectly executes the core learning loop. A user will be able to:

1.  **Sign up** for an account using their email or Google account.
2.  **Create** a new, private deck for a specific language.
3.  **Add** multiple flashcards to that deck, with text on the front and back.
4.  **Hear** automatically generated audio pronunciation for the text on their cards.
5.  **Initiate** a review session where the app intelligently presents cards due for study based on the spaced repetition algorithm.
6.  **Self-assess** their recall for each card, which updates its next review date.
7.  **View** their current study streak on a simple dashboard.

The MVP will be fully functional and provide immediate value by helping users learn and retain vocabulary more effectively than traditional methods. It intentionally excludes public deck browsing, advanced gamification, and rich media to ensure a timely launch with a high-quality core experience.