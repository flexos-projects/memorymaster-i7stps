---
id: pages-sitemap
title: Site Map & User Journeys
description: Outlines the application's page structure, site map, primary navigation patterns, and key user journeys.
type: doc
subtype: core
status: draft
sequence: 3
tags:
  - architecture
  - ux
  - pages
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## 1. Site Map

This hierarchical map details the page structure of MemoryMaster.

*   **Public Routes (Unauthenticated)**
    *   `/home` - **Landing Page**: The main marketing and entry point.
    *   `/auth` - **Login / Signup Page**: A single, unified page for all authentication actions.

*   **Private Routes (Authenticated)**
    *   `/dashboard` - **Dashboard**: The user's personalized home base.
    *   `/decks` - **My Decks**: A list of all user-owned and subscribed decks.
        *   `/decks/:id` - **Deck Detail**: View and manage the cards within a specific deck.
            *   `/decks/:deckId/cards/:cardId?` - **Card Editor**: A dedicated page/modal for creating or editing a card.
    *   `/review` - **Review Session**: The focused learning interface. Can be global (`/review`) or deck-specific (`/review/:deckId`).
    *   `/explore` - **Explore Public Decks**: The community hub for discovering new content.
    *   `/settings` - **Settings**: User profile, preferences, and account management.

## 2. Page Inventory & Key Components

*   **Dashboard (`/dashboard`)**: The first screen after login. Its primary purpose is to drive engagement with the core learning loop. Key components include a prominent 'Daily Streak' counter, a summary of total cards due for review, and a large 'Start Review' call-to-action button. It will also feature a list of recently worked-on decks.

*   **My Decks (`/decks`)**: This is the user's personal library. The page will display a grid or list of deck cards. Each card must show the deck name, language, mastery percentage, and number of cards. Actions on each deck will include 'Review', 'Edit', and a menu for 'Share'/'Delete'. A floating action button with a '+' icon will trigger the 'Create New Deck' flow.

*   **Review Session (`/review/:deckId?`)**: This is the most critical interactive page. The design must be minimal and distraction-free. The flow is paramount: (1) Display front of card. (2) User action to reveal back. (3) Display back of card with front still visible for context. (4) Prominently display the self-assessment buttons ('Hard', 'Good', 'Easy'). A progress bar showing 'X of Y cards reviewed' will be visible at the top.

*   **Explore Public Decks (`/explore`)**: The discovery engine. This page will feature a search bar and filters for language and tags. Decks will be displayed in a card format showing creator, language, and number of subscribers. Clicking a deck should open a preview modal showing 5-10 sample cards before the user commits to adding it.

## 3. Navigation

*   **Unauthenticated:** A simple header on the `/home` page with 'Features', 'Login', and a primary 'Sign Up' button.
*   **Authenticated (Mobile-First):** As specified in the design concept, a `bottom-tabs` navigation bar will be the primary method of navigation. It will contain four icons:
    1.  **Dashboard** (Home icon)
    2.  **My Decks** (Layers icon)
    3.  **Explore** (Compass icon)
    4.  **Settings** (User avatar or Gear icon)
*   **Authenticated (Desktop):** The layout will progressively enhance to a persistent left-hand sidebar with the same navigation items, including text labels for clarity.

## 4. Key User Journey: Onboarding & First Review

This journey describes the path of a new user, 'Sofia', from signup to her first successful study session.

1.  **Landing & Signup:** Sofia arrives at `/home`, is convinced by the value proposition, and clicks 'Sign Up'. She is taken to `/auth` and registers using her Google account.
2.  **Dashboard & Prompt:** After authentication, she is redirected to `/dashboard`. A welcome modal appears, prompting her to create her first deck.
3.  **Deck & Card Creation:** She clicks 'Create Deck', enters 'Spanish Travel Phrases', and is taken to the empty Deck Detail page `/decks/:id`. She clicks 'Add Card' which opens the `/decks/:deckId/cards/new` editor. She adds her first card: Front='Hola', Back='Hello'. She saves it.
4.  **Initiating Review:** Back on the dashboard, her new deck is visible and the 'Start Review' button is active. She clicks it.
5.  **First Session:** She enters the `/review` page. The card 'Hola' is shown. She clicks 'Show Answer', sees 'Hello', and confidently clicks 'Easy'.
6.  **Session Complete:** A confirmation screen congratulates her on completing her first review. Her daily streak is now '1'. She is returned to the dashboard, feeling accomplished and ready to add more cards.