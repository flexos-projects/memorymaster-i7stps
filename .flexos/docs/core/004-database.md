---
id: database-schema
title: Database Schema & Security
description: Details the Firestore collection schemas, data relationships, required indexes, and security rules for MemoryMaster.
type: doc
subtype: core
status: draft
sequence: 4
tags:
  - database
  - firestore
  - architecture
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## 1. Database Choice: Firestore

We will use Firestore as our primary database. Its serverless nature, real-time capabilities, and robust client-side SDKs make it a perfect fit for our interactive, mobile-first application. The document-based model is flexible enough to handle our data structures.

## 2. Collections Schema

**`users`**
*   Stores public profile and application settings for each user.
*   Document ID: `Firebase Auth UID`
    *   `email` (string, required)
    *   `displayName` (string)
    *   `profilePhotoUrl` (string, URL)
    *   `learningLanguages` (array of strings, e.g., `['es', 'jp']`)
    *   `createdAt` (timestamp)

**`decks`**
*   Contains metadata for each user-created flashcard deck.
*   Document ID: `auto-generated`
    *   `ownerId` (string, UID, required) - *Foreign Key to `users`*
    *   `name` (string, required)
    *   `description` (string)
    *   `targetLanguage` (string, ISO 639-1 code, required)
    *   `cardCount` (number, integer) - *Denormalized for performance*
    *   `isPublic` (boolean, default: `false`)
    *   `createdAt` (timestamp)
    *   `updatedAt` (timestamp)

**`cards`**
*   Stores the content for each individual flashcard.
*   Document ID: `auto-generated`
    *   `deckId` (string, required) - *Foreign Key to `decks`*
    *   `ownerId` (string, UID, required) - *Denormalized for security rules*
    *   `frontContent` (object) - *Tiptap JSON format for rich text*
    *   `backContent` (object) - *Tiptap JSON format for rich text*
    *   `frontAudioUrl` (string, URL)
    *   `backAudioUrl` (string, URL)
    *   `imageUrl` (string, URL)
    *   `createdAt` (timestamp)

**`card_progress`**
*   The core of the SRS. Tracks each user's progress on each card they study.
*   Document ID: `auto-generated` (or a composite `userId_cardId`)
    *   `userId` (string, UID, required) - *Foreign Key to `users`*
    *   `cardId` (string, required) - *Foreign Key to `cards`*
    *   `deckId` (string, required) - *Denormalized for efficient queries*
    *   `nextReviewAt` (timestamp, required) - *The core scheduling field*
    *   `easeFactor` (number, float, default: 2.5)
    *   `repetitions` (number, integer, default: 0)
    *   `interval` (number, days, default: 0)
    *   `lastReviewedAt` (timestamp)

## 3. Data Relationships

*   **User to Decks:** One-to-Many (`users.id` -> `decks.ownerId`). A user can own many decks.
*   **Deck to Cards:** One-to-Many (`decks.id` -> `cards.deckId`). A deck contains many cards.
*   **User to Card Progress:** One-to-Many (`users.id` -> `card_progress.userId`). A user has progress data for many cards.

## 4. Firestore Indexes

To support our primary query patterns, the following composite indexes will be required:

1.  **`card_progress` Collection:**
    *   `userId` (asc), `nextReviewAt` (asc) - Essential for efficiently fetching all cards due for review for a given user.
2.  **`decks` Collection:**
    *   `isPublic` (asc), `createdAt` (desc) - To query for all public decks, sorted by most recent, for the 'Explore' page.
3.  **`cards` Collection:**
    *   `deckId` (asc), `createdAt` (asc) - To fetch all cards for a specific deck in the order they were created.

## 5. Security Rules

Security is paramount. The following rules will be implemented to protect user data:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users can read and write their own user document.
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }

    // Decks can be read by anyone if public, but only written by the owner.
    match /decks/{deckId} {
      allow read: if resource.data.isPublic == true || request.auth.uid == resource.data.ownerId;
      allow create: if request.auth.uid == request.resource.data.ownerId;
      allow update, delete: if request.auth.uid == resource.data.ownerId;
    }

    // Cards can be read if the user has access to the parent deck. Written only by owner.
    match /cards/{cardId} {
      allow read: if get(/databases/$(database)/documents/decks/$(resource.data.deckId)).data.isPublic == true || request.auth.uid == get(/databases/$(database)/documents/decks/$(resource.data.deckId)).data.ownerId;
      allow create, update, delete: if request.auth.uid == resource.data.ownerId;
    }

    // Users can only access their own progress documents.
    match /card_progress/{progressId} {
      allow read, write: if request.auth.uid == resource.data.userId;
      allow create: if request.auth.uid == request.resource.data.userId;
    }
  }
}
```