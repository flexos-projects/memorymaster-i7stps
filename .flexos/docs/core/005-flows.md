---
id: user-system-flows
title: User & System Flows
description: Details the critical user and system interaction flows, including the spaced repetition logic and error handling considerations.
type: doc
subtype: core
status: draft
sequence: 5
tags:
  - flows
  - ux
  - architecture
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## 1. Flow: Daily Review Session & SRS Logic

This is the core learning loop of MemoryMaster. It details the interaction between the user and the spaced repetition system (SRS).

*   **Trigger:** User clicks the 'Start Review' button on the `/dashboard` page.
*   **Actor:** System (Frontend Client)
    1.  **Query for Due Cards:** Execute a Firestore query on the `card_progress` collection where `userId` equals the current user's UID and `nextReviewAt` is less than or equal to the current timestamp. Order by `nextReviewAt` ascending.
    2.  **Fetch Card Content:** For each `card_progress` document returned, fetch the corresponding full document from the `cards` collection using its `cardId`.
    3.  **Initialize Session:** Create a queue of card objects in the client-side state (e.g., in a Pinia store). Transition the UI to the `/review` page.
*   **Actor:** User & System (In Session)
    1.  **Display Card:** The system displays the `frontContent` of the first card in the queue. If `frontAudioUrl` exists, the audio is played automatically (based on user settings).
    2.  **User Recall:** The user attempts to recall the `backContent`.
    3.  **Reveal Answer:** User clicks 'Show Answer'. The UI transitions to show the `backContent`.
    4.  **Self-Assessment:** The user clicks one of three buttons:
        *   **'Hard' (Recall failed):** The system resets the card's `repetitions` in `card_progress` to 0. The `interval` is set to a minimal value (e.g., 1 minute). The card is often re-queued for the end of the current session.
        *   **'Good' (Correct recall with some effort):** The system increments `repetitions`. It calculates the `newInterval` using the SM-2 formula: `newInterval = lastInterval * easeFactor`. The `easeFactor` remains unchanged.
        *   **'Easy' (Correct, effortless recall):** The system increments `repetitions`. It calculates the `newInterval` similarly to 'Good' but also slightly increases the `easeFactor` (e.g., `easeFactor + 0.15`), making future intervals grow faster.
    5.  **Update Progress:** The system writes the updated `nextReviewAt`, `repetitions`, `interval`, and `easeFactor` back to the corresponding `card_progress` document in Firestore.
    6.  **Loop:** The system presents the next card in the queue. This continues until the queue is empty.
*   **End of Session:**
    1.  **Summary Screen:** The system displays a summary screen congratulating the user, showing the number of cards reviewed, and updating their daily streak.
    2.  **Redirect:** The user is redirected back to the `/dashboard`.

## 2. Flow: Exploring and Adding a Public Deck

This flow describes how a user discovers and incorporates community content into their learning.

*   **Trigger:** User navigates to the `/explore` page.
*   **Steps:**
    1.  **Discovery:** The system queries the `decks` collection for all documents where `isPublic == true`, sorted by a popularity metric or creation date. The user can further filter by `targetLanguage` or search by name.
    2.  **Preview:** The user clicks on a deck. A modal appears, showing the deck's description, creator, and a sample of 5-10 cards (read-only) to give a feel for the content quality.
    3.  **Add to My Decks:** The user clicks the 'Add to My Decks' button.
    4.  **System Action (Backend/Client):** This is a critical step. The system does **not** simply bookmark the deck. To enable personalized progress tracking, it must perform the following actions:
        a. Create a new document in the `user_deck_subscriptions` collection to link the `userId` and the public `deckId`.
        b. Fetch all `cards` associated with the public `deckId`.
        c. For **each** card, create a new `card_progress` document for the current user, setting `userId`, `cardId`, and `deckId`. The `nextReviewAt` is set to the current timestamp, making all new cards immediately available for the user's review.
    5.  **Confirmation:** The UI shows a confirmation message (e.g., 'Deck added!') and the button changes to 'In Your Decks'. The user can now find this deck on their `/decks` page and its cards will appear in their review sessions.

## 3. Error Handling Considerations

*   **Offline During Review:** If the user's connection drops mid-session, the client should store progress updates locally (e.g., in `localStorage`). Once the connection is restored, a batch write should be performed to sync the changes to Firestore.
*   **TTS API Failure:** If the text-to-speech generation fails for a card, the UI should not hang. The audio play button should be disabled, and a small tooltip on hover can indicate 'Audio unavailable'.
*   **Concurrent Data Modification:** While rare for this app, Firestore's optimistic concurrency handles most cases. For sensitive operations like deleting a deck, we can use Firestore Transactions to ensure all associated cards and progress data are deleted atomically.