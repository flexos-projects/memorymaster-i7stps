---
id: design-system
title: Design System & Brand Identity
description: Establishes the brand voice, color system, typography, core components, and accessibility standards for MemoryMaster.
type: doc
subtype: core
status: draft
sequence: 6
tags:
  - design
  - ui
  - accessibility
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

## 1. Brand Voice & Tone

MemoryMaster's voice is **Encouraging, Intelligent, and Clear**. We are a learning companion, not a strict teacher. Our tone should be motivational and celebrate progress, no matter how small.

*   **Use words like:** "Mastery," "Progress," "Keep it up!," "Discover," "You've learned..."
*   **Avoid:** Dry, academic language or overly complex jargon.
*   **Examples:**
    *   **Good:** "You've completed your review for today. Great job! Your streak is now 5 days."
    *   **Bad:** "Review session terminated. User progress has been updated."

## 2. Color System

Our color palette is vibrant and modern, designed to create an engaging user experience while ensuring clarity and accessibility.

*   **Primary:** `#ec4899` (Pink-500) - Used for primary calls-to-action (e.g., 'Start Review', 'Save Card'), active navigation states, and major highlights.
*   **Accent:** `#6366f1` (Indigo-500) - Used for secondary actions, links, informational icons, and highlighting user input fields.
*   **Neutral (Grays):**
    *   `#1f2937` (Gray-800): For primary headlines and body text.
    *   `#6b7280` (Gray-500): For secondary text and icons.
    *   `#d1d5db` (Gray-300): For borders and dividers.
    *   `#f9fafb` (Gray-50): For page backgrounds.
*   **Feedback Colors:**
    *   **Success:** `#10b981` (Green-500) - For success messages, validation.
    *   **Warning:** `#f59e0b` (Amber-500) - For non-critical alerts.
    *   **Error:** `#ef4444` (Red-500) - For error messages and destructive actions.

## 3. Typography

We will use the **Inter** font family for its exceptional readability on digital screens at all sizes.

*   **Font Family:** `Inter`, with a fallback to standard sans-serif system fonts.
*   **Typographic Scale (based on 16px root):**
    *   `H1` (Heading 1): 2.25rem (36px), font-weight: 700
    *   `H2` (Heading 2): 1.75rem (28px), font-weight: 700
    *   `H3` (Heading 3): 1.25rem (20px), font-weight: 600
    *   `Body`: 1rem (16px), font-weight: 400
    *   `Caption`: 0.875rem (14px), font-weight: 400

## 4. Core Components

A consistent component library is key to a cohesive user experience.

*   **Buttons:**
    *   **Primary:** Solid `#ec4899` background with white text.
    *   **Secondary:** Transparent background with an `#6366f1` border and text.
    *   **Tertiary (Ghost):** No background or border, with text color based on context.
    *   **States:** All buttons must have defined `hover`, `focus`, and `disabled` states with subtle transitions.
*   **Deck Card:** The standard container for displaying a deck in a list. It will have a light background, subtle box-shadow, rounded corners (8px), and clearly defined areas for the deck name, mastery level, and action buttons.
*   **Flashcard (Review View):** A large, centered card with a distinct front and back design. A flip animation will be used for the reveal. The content area will be optimized for readability with large typography.
*   **Input Fields:** Clean, simple inputs with a bottom border or light gray background. The border color will change to the accent color (`#6366f1`) on focus and the error color (`#ef4444`) on validation failure.

## 5. Accessibility (A11y)

Accessibility is a primary requirement, not an afterthought.

*   **Color Contrast:** All text and UI elements must meet a minimum WCAG AA contrast ratio of 4.5:1 against their background. The primary pink (`#ec4899`) on white must be verified and potentially darkened if it fails.
*   **Keyboard Navigation:** All interactive elements, including buttons, links, form fields, and flashcard actions, must be focusable and operable using the Tab and Enter/Space keys.
*   **Screen Readers:** We will use semantic HTML5 (`<main>`, `<nav>`, `<button>`) to provide context. ARIA attributes (e.g., `aria-label`, `aria-live`) will be used to announce dynamic content changes, especially during the review session (e.g., "Card revealed," "Answer was correct").
*   **Reduced Motion:** All animations, such as the card flip, will respect the `prefers-reduced-motion` media query, replacing them with a simple cross-fade for users who are sensitive to motion.