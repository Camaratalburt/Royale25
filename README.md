# royale25

**Casino Royale: The 25th Gala** — a bespoke event platform for a private black-tie evening.

> 19 September 2026 · Birmingham · ProdByCamz Presents

---

## What this is

This repo hosts the guest-facing web platform for the event. Guests RSVP, get their ticket, see who else is coming, send reactions (which unlock private chats when mutual), suggest tracks for the playlist, and contribute to the album.

## Current state

**Prototype** — a single self-contained HTML file with hash-based routing, localStorage persistence, and mock data. Seven views:

- `#landing` — cover page with countdown, CTA
- `#rsvp` — 6-screen flow with dual auth (email OR phone), plus-one capture, profile setup
- `#dashboard` — ticket with QR, countdown, module cards, activity feed
- `#guests` — The Hall: stats, filter tabs, search, 18 seeded guest cards with reaction system
- `#chat` — Matches: sidebar + chat pane for mutually-reacted guests
- `#playlist` — approved tracks + submission queue with 30s preview bar
- `#album` — masonry photo grid with upload flow, moderation queue, full-screen lightbox

All data lives in localStorage under `cr25_*` keys. Nothing hits a server yet.

## Stack

**Today (prototype):** Single-file HTML + vanilla JS + CSS. No build step. No dependencies except Google Fonts.

**Planned (production):**
- **Frontend:** Next.js 15 (App Router) + React + TypeScript + Tailwind
- **Backend:** Supabase (Postgres, Auth, Storage, Edge Functions, Realtime)
- **Auth:** Supabase Auth with magic-link email + Twilio SMS OTP
- **Images:** Cloudinary (uploads, transformations, delivery)
- **Music:** Spotify Web API (search, 30s preview, playlist export)
- **Notifications:** Resend (email) + Twilio (SMS) + Web Push
- **Hosting:** Vercel (this repo)
- **Domain:** royale25.co.uk (Namecheap)

## Local preview

No build step. Just open in a browser:

```sh
open index.html
```

Or serve locally with any static server:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy

This repo is linked to Vercel. Every push to `main` triggers a deploy:

```sh
git add index.html
git commit -m "Update: [describe change]"
git push
```

Vercel picks it up within ~20 seconds and the live URL updates.

## Testing the prototype

To skip the RSVP gate during testing, either:

1. Complete the RSVP flow once (state persists in localStorage), **or**
2. Visit `#dashboard` and click the "skip RSVP" link on the access gate

To reset state and start fresh:

```js
// In browser console:
localStorage.clear();
location.reload();
```

## Key decisions locked in the brief

- Dual auth at RSVP: guests give email OR phone, then the opposite contact on their profile screen
- Plus-one: 1 included automatically, up to 3 additional via admin approval queue
- Individual tickets with unique QR codes (not shared with plus-ones)
- Guest interaction = reactions + mutual-match chat (not open DMs)
- 5 reaction types: Hello / Cheers / Tables / Snap / Chat
- Guest-facing views hide Declined/Pending guests — admin-only
- Platform stays live indefinitely after the event (owner closes manually)

## File layout

```
royale25/
├── index.html              # The entire prototype — all views, routing, state
└── README.md               # This file
```

Yes, one file. The production migration to Next.js will split this into proper components, pages, and server logic.

## Project credits

**Host / Director / Designer:** Camara Talburt
**Agency:** Assonance Ltd
**Producer brand:** ProdByCamz

## License

Private. Not for redistribution.
