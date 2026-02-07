---
id: technical-architecture
title: Technical Architecture & Stack
description: Documents the high-level architecture, technology stack, API design, and deployment strategy for MemoryMaster.
type: doc
subtype: core
status: draft
sequence: 7
tags:
  - architecture
  - tech-stack
  - firebase
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## 1. High-Level Architecture

MemoryMaster is architected as a modern web application leveraging a serverless backend. This approach maximizes scalability, minimizes operational overhead, and accelerates development.

*   **Frontend:** A Single Page Application (SPA) built with **Nuxt 4 (Vue 3)**. Authenticated sections of the app will be client-side rendered for a fast, interactive experience. Public-facing pages like the landing page will be server-side rendered (SSR) or statically generated (SSG) for optimal SEO and initial load performance.
*   **Backend:** A collection of **Firebase** services providing a comprehensive, serverless backend.
    *   **Authentication:** Firebase Authentication will manage user identity, supporting email/password and OAuth providers (Google, Apple).
    *   **Database:** **Firestore** will serve as our NoSQL database, providing real-time data synchronization and flexible data modeling.
    *   **File Storage:** **Firebase Storage** will be used for user-uploaded assets, specifically images and custom audio recordings for flashcards.

## 2. Technology Stack

*   **Framework:** **Nuxt 4** - Chosen for its powerful file-based routing, component auto-imports, server-side capabilities, and overall excellent developer experience within the Vue ecosystem.
*   **State Management:** **Pinia** - The official, lightweight, and type-safe state management library for Vue. It's ideal for managing session state, user data, and the review queue.
*   **UI & Styling:** **Tailwind CSS** - A utility-first CSS framework that will allow us to rapidly build a consistent and custom design system directly in our markup.
*   **Rich Text Editing:** **Tiptap** - A headless, framework-agnostic rich text editor that provides full control over the look and feel of our card editor. It outputs structured JSON, which is ideal for storing in Firestore.
*   **Audio Playback:** **Howler.js** - A robust JavaScript audio library that abstracts away the complexities of the Web Audio API, ensuring reliable audio playback across all modern browsers.

## 3. API & Server Routes

While the majority of data interaction will occur directly between the client and Firestore via the Firebase SDK, certain operations require a secure server-side environment. We will use Nuxt's built-in server routes for these cases.

*   **`POST /api/tts` (Text-to-Speech Generation)**
    *   **Purpose:** To prevent exposing our Google Cloud API keys on the client.
    *   **Flow:**
        1.  The client sends a request to this endpoint with `{ text: 'Hola', language: 'es-ES' }`.
        2.  The Nuxt server route receives the request, validates the input, and authenticates the user.
        3.  The server makes a call to the Google Cloud Text-to-Speech API using a service account key stored securely as an environment variable.
        4.  Google Cloud returns the generated audio file (e.g., an MP3).
        5.  The server saves this file to a publicly accessible location in Firebase Storage.
        6.  The server responds to the client with the public URL of the stored audio file.
        7.  The client then saves this URL to the `frontAudioUrl` or `backAudioUrl` field in the `cards` document.

## 4. Deployment & DevOps

*   **Hosting:** The application will be deployed to **Firebase Hosting**. This provides a global CDN, automatic SSL, and seamless integration with our Firebase backend. Custom domains will be configured through Firebase.
*   **CI/CD Pipeline:** We will use **GitHub Actions** for continuous integration and deployment.
    *   **Trigger:** A push or merge to the `main` branch.
    *   **Workflow:**
        1.  **Checkout Code:** The workflow checks out the latest commit.
        2.  **Setup Node.js:** Installs the required Node.js version.
        3.  **Install Dependencies:** Runs `npm install` to fetch all packages.
        4.  **Lint & Test:** Runs `npm run lint` and `npm run test` to ensure code quality and prevent regressions.
        5.  **Build:** Executes `npm run build` to generate the production-ready Nuxt application.
        6.  **Deploy:** Uses the official `firebase-tools` action to deploy the contents of the build directory to Firebase Hosting.

This automated pipeline ensures that every change to the main branch is tested and deployed quickly and reliably, allowing for rapid iteration.