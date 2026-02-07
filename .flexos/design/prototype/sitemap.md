---
id: prototype-sitemap
title: "Prototype Sitemap"
description: "Route map for prototype pages"
type: design
subtype: sitemap
status: draft
sequence: 0
tags: [prototype, routing]
relatesTo: []
createdAt: "2026-02-07T03:34:50.682Z"
updatedAt: "2026-02-07T03:34:50.682Z"
---

# Prototype Sitemap

<flex_block type="config">
{
  "routes": {
    "/home": "landing-page-v1.html",
    "/auth": "auth-page-v1.html",
    "/dashboard": "dashboard-page-v1.html",
    "/decks": "my-decks-page-v1.html",
    "/decks/:id": "deck-detail-page-v1.html",
    "/decks/:deckId/cards/:cardId?": "card-editor-page-v1.html",
    "/review/:deckId?": "review-session-page-v1.html",
    "/explore": "explore-decks-page-v1.html",
    "/settings": "settings-page-v1.html"
  },
  "fallback": "404.html",
  "pages": [
    {
      "id": "landing-page",
      "route": "/home",
      "file": "landing-page-v1.html",
      "auth": true
    },
    {
      "id": "auth-page",
      "route": "/auth",
      "file": "auth-page-v1.html",
      "auth": true
    },
    {
      "id": "dashboard-page",
      "route": "/dashboard",
      "file": "dashboard-page-v1.html",
      "auth": true
    },
    {
      "id": "my-decks-page",
      "route": "/decks",
      "file": "my-decks-page-v1.html",
      "auth": true
    },
    {
      "id": "deck-detail-page",
      "route": "/decks/:id",
      "file": "deck-detail-page-v1.html",
      "auth": true
    },
    {
      "id": "card-editor-page",
      "route": "/decks/:deckId/cards/:cardId?",
      "file": "card-editor-page-v1.html",
      "auth": true
    },
    {
      "id": "review-session-page",
      "route": "/review/:deckId?",
      "file": "review-session-page-v1.html",
      "auth": true
    },
    {
      "id": "explore-decks-page",
      "route": "/explore",
      "file": "explore-decks-page-v1.html",
      "auth": true
    },
    {
      "id": "settings-page",
      "route": "/settings",
      "file": "settings-page-v1.html",
      "auth": true
    }
  ]
}
</flex_block>

## Routes

| Route | File | Auth |
|-------|------|------|
| /home | landing-page-v1.html | Yes |
| /auth | auth-page-v1.html | Yes |
| /dashboard | dashboard-page-v1.html | Yes |
| /decks | my-decks-page-v1.html | Yes |
| /decks/:id | deck-detail-page-v1.html | Yes |
| /decks/:deckId/cards/:cardId? | card-editor-page-v1.html | Yes |
| /review/:deckId? | review-session-page-v1.html | Yes |
| /explore | explore-decks-page-v1.html | Yes |
| /settings | settings-page-v1.html | Yes |

---

*Prototype HTML files pending AI generation*
