# CLAUDE.md — Context for Claude Code

This file gives Claude Code (or any AI assistant) the full context needed to work on this repository productively. Read this in full before making changes.

---

## Project identity

**Name:** Casino Royale: The 25th Gala
**What it is:** A bespoke event platform for a private black-tie 25th birthday party
**Host / Owner:** Camara Talburt (ProdByCamz / Assonance Ltd)
**Event date:** 19 September 2026 (20 September as backup)
**Venue:** Private venue in Birmingham (negotiating with The Bond/Canopy — minimum spend model, not flat hire fee)
**Capacity:** 140–160 guests, black tie standing party
**Format:** Canapés, champagne reception, open bar, DJ + live band + casino tables
**Flyer tagline:** "ProdByCamz Presents..." (flyers only, not platform branding)
**Monogram:** C · T · XXV
**Production domain:** royale25.co.uk (to be registered on Namecheap before launch)

---

## Current state

**A single-file HTML prototype**, deployed to Vercel. One `index.html` contains all seven views, routed via URL hash (`#landing`, `#rsvp`, `#dashboard`, `#guests`, `#chat`, `#playlist`, `#album`). Data persists in `localStorage` under `cr25_*` keys. No backend yet.

The file is ~313KB, fully self-contained, uses only Google Fonts as an external dependency.

---

## Views and their purpose

1. **#landing** — Cover page. Countdown to 19/09/2026 19:00 BST. "Place Your Bet" CTA leads to RSVP. Hero title with gold gradient over obsidian.
2. **#rsvp** — 6-screen flow:
   - Screen 1: Auth method choice (email or phone)
   - Screen 2: OTP verify
   - Screen 3: 3 playing-card RSVP decision (I'm In / Maybe / Can't Make It)
   - Screen 4: Plus-one — guests capture plus-one details OR defer with "decide later"
   - Screen 5: Profile setup — photo, name, bio, opposite contact method, optional socials
   - Screen 6: Confirmation with animated wax seal
   - Screen 7: Decline exit screen
3. **#dashboard** — Main platform landing. 6-second cinematic welcome animation on first entry. Individual ticket with QR code (no seat, just "General"). Countdown bar. 4 module cards (Guests / Playlist / Album / Venue). Activity feed with 5 seeded items.
4. **#guests** ("The Hall") — Stats (Attending / Undecided — Declined never shown to guests), filter tabs, search, sort, masonry of 18 seeded guest cards. Profile modal shows bio, socials, plus-one, and 5-reaction grid (Hello / Cheers / Tables / Snap / Chat).
5. **#chat** ("Matches") — Sidebar of mutual-match conversations + chat pane. Match seal at top of each conversation. Gold-tinted own messages, surface-grey theirs, day dividers. Only unlocks when both guests have sent reactions to each other.
6. **#playlist** — Stats row, dual tabs (The Playlist / My Submissions), 8 vibe filter pills, search-to-submit flow with mock Spotify catalogue. 30-second preview bar slides up from bottom. Submission status pills (PENDING / APPROVED / REJECTED). 18 seeded approved tracks.
7. **#album** — Masonry photo grid (3 cols desktop / 2 mobile). 24 seeded photos with subtle gold crown badges on host posts. Upload modal with dropzone. Moderation queue with status pills. Full-screen lightbox with prev/next nav, keyboard support (ESC, arrows), counter, caption bar.

---

## Design system

**Palette:**
- `--obsidian: #0A0A0A` (background)
- `--charcoal: #131210`
- `--surface: #1A1814`
- `--surface-raised: #221F19`
- `--gold: #D4AF37` (primary brand accent)
- `--gold-light: #E8D5A3`
- `--gold-dark: #8A6F24`
- `--cream: #F5EFE0` (primary text)
- `--cream-muted: #B8AC94`
- `--burgundy: #4A0E1C`
- `--success: #5DCAA5`, `--warning: #EF9F27`, `--danger: #D85A30`

**Typography:**
- **Cinzel** (400-700) — all-caps titles, labels, eyebrow text, letter-spaced
- **Cormorant Garamond** (300-500, italic heavily used) — poetic secondary, captions, empathetic messaging
- **DM Sans** (300-500) — UI body text, inputs, messages, neutral content

**Iconography & ornament:**
- SVG art-deco corner ornaments on cards and ticket
- Playing card suits (♠ ♦ ♣ ♥) as accent symbols
- Subtle grain overlay and vignette via radial gradients
- Gold particle drift (subtle animated background element)

---

## Locked product decisions (from original brief — do not revisit without explicit approval)

- **Dual auth at RSVP:** guests provide email OR phone initially, then the opposite contact on profile screen. Production will use Supabase Auth (magic-link email) + Twilio (SMS OTP).
- **Plus-one rules:** 1 additional included automatically. Up to 3 more require admin approval with mandatory reason.
- **Platform stays live indefinitely.** Owner manually closes.
- **Multi-admin with 3 role tiers:** Super Admin (Camara only), Admin (committee members), Moderator (photo approval + QR door scanning only).
- **Tickets are individual, not shared.** Each guest (including plus-ones) gets unique QR tokens. Ticket is unveiled AFTER entering the platform, not at RSVP confirmation.
- **Guest-facing views hide Declined and Pending guests.** Only Attending and Maybe are shown. Admin sees everything.
- **Guest interaction = reactions + mutual-match chat.** 5 one-tap reactions (Hello / Cheers / Tables / Snap / Chat). Private DM unlocks only when both guests have reacted to each other. No open DMs.
- **Party tone in copy:** "Are You In?", "I'm In / Maybe / Can't Make It", "Show this at the door", "See you on the night". Avoid overly formal language — it's a standing party, not a seated gala.

---

## Stack

**Today (prototype):**
- Single-file HTML + vanilla JS + CSS
- Hash-based routing, no build step
- `localStorage` for all state

**Planned (production migration):**
- **Frontend:** Next.js 15 (App Router) + React + TypeScript + Tailwind
- **Backend:** Supabase (Postgres, Auth, Storage, Edge Functions, Realtime)
- **Auth:** Supabase Auth magic-link + Twilio SMS OTP
- **Images:** Cloudinary (uploads, transformations, CDN delivery)
- **Music:** Spotify Web API (search, 30s preview via `preview_url`, playlist export)
- **Notifications:** Resend (email) + Twilio (SMS) + Web Push API
- **Hosting:** Vercel (already deployed — the Vercel project is linked)
- **Domain:** royale25.co.uk (via Namecheap → Vercel DNS once registered)

---

## File layout

```
royale25/
├── index.html       # The entire prototype
├── README.md        # Public-facing repo documentation
├── CLAUDE.md        # This file — for AI collaborators
└── .gitignore
```

Yes, one HTML file. Production migration will break this into proper Next.js components/pages/server actions.

---

## How to work on this prototype

**Make edits in `index.html`.** Every view's CSS and JS is in this single file. The bundler concept is gone at this point — we ship this file directly.

**Local preview:**
```sh
open index.html
```
Or serve locally:
```sh
python3 -m http.server 8000
# then http://localhost:8000
```

**Testing state:**
To skip the RSVP gate:
- Complete the flow once (persists in localStorage), OR
- Click "skip RSVP" link on the access gate (prototype-only)

To reset:
```js
// In browser console:
localStorage.clear();
location.reload();
```

**Deploy workflow:**
The repo is connected to Vercel. Every `git push` to `main` auto-deploys.

```sh
git add index.html
git commit -m "Describe what changed"
git push
# Vercel deploys in ~20 seconds
```

For production promotion (clean `royale25.vercel.app` URL):
```sh
vercel --prod
```

---

## Routing architecture

A hash-based router lives in the `<script>` block near the bottom of `index.html`. The `VIEWS` array lists all valid view names. Navigation happens via:

1. **Explicit attribute:** `<a href="#X" data-nav="X">` — router intercepts click
2. **Plain hash link:** `<a href="#X">` — router intercepts if hash matches a known view

Both selectors are caught by the document-level click handler. This dual behaviour was added after a bug where the landing page's "Place Your Bet" CTA had `href="#rsvp"` but no `data-nav` attribute, so the router ignored it.

Each view has:
- An `init` function that runs once when the view is first shown (`viewInitializers[name]`)
- An optional `reentry` function that runs each subsequent time (`viewReentry[name]`)

---

## LocalStorage keys

| Key | Purpose |
|---|---|
| `cr25_rsvp_choice` | `"yes"` / `"maybe"` / `"no"` — access gate |
| `cr25_rsvp_complete` | `"true"` once profile is set up |
| `cr25_reactions_sent` | `{ [guestId]: { [reactionType]: true } }` |
| `cr25_reactions_received` | Same shape — who reacted to you |
| `cr25_matches` | `[guestId, guestId, ...]` — mutual matches |
| `cr25_messages` | Chat messages per match |
| `cr25_submissions` | User's playlist track submissions |
| `cr25_my_photos` | User's uploaded album photos |

---

## Demo / seed data

The prototype ships with pre-seeded state for testing:
- 2 pre-seeded matches: Adan Hussain (guest id 3), Liam Shaw (guest id 10)
- Received-but-not-sent reactions from: Olivia Bennett (id 5), Amaka Eze (id 13) — these are "reacted to you, waiting for your move" so you can trigger a match live
- 18 guest cards in The Hall with bios, status, plus-ones, socials
- 18 approved playlist tracks (mix of Amapiano, Afrobeats, R&B, Classics)
- 24 album photos with gradient placeholders
- 1 pending submission in "My Submissions" for testing the moderation UI

---

## What to build next (open work)

### Priority 1: Admin dashboard
Not yet built. Should live at `#admin` (role-gated). Needs:
- RSVP status queue (view by status)
- Plus-one approval queue with reason display
- Playlist moderation (approve / reject with note)
- Photo moderation (approve / reject with note)
- Admin role management
- Notification composer (send email or SMS blast)
- Analytics summary (RSVP rate, playlist contributions, photo uploads)

### Priority 2: Profile & settings
- Personal profile page at `#profile` (edit photo, bio, contact methods, socials)
- Settings at `#settings` (notification preferences, export data, account deletion)

### Priority 3: Notifications system
- Email templates via Resend
- SMS templates via Twilio
- Web Push registration + service worker
- Per-event preferences (mute match notifications, mute playlist activity, etc.)

### Priority 4: Production migration to Next.js + Supabase
- Schema design (guests, rsvps, reactions, matches, messages, playlist_tracks, playlist_submissions, photos, photo_submissions, admin_users, audit_log)
- Row-level security policies
- Edge Functions for QR verification, notification sending, Spotify API integration
- Realtime subscriptions for reactions, new messages, new photos
- Move localStorage state to DB-backed state

### Priority 5: Brand assets
- Monogram SVG (reusable across invite, ticket, emails)
- Favicon (the C·T·XXV mark)
- OG image for social sharing preview
- Email template headers

---

## Known quirks

- The welcome animation on `#dashboard` only plays once per session — subsequent visits skip straight to the revealed state via `viewReentry`. This is intentional.
- `#chat` uses fixed-viewport layout (`body.view-chat { overflow: hidden; height: 100vh }`). Other views use normal scroll. Do not propagate chat's layout style to other views.
- Mobile nav drawer uses hamburger (`.mobile-nav-toggle`) at ≤768px. Desktop uses inline `.nav-items`.
- The prototype's "skip RSVP" link on the access gate bypasses auth — MUST be removed before production launch.

---

## Code conventions

- **CSS:** Use the defined CSS variables (`var(--gold)` etc). Do not hardcode palette colours.
- **Typography:** Use the three font families purposefully — Cinzel for display/labels, Cormorant for poetry/empathy, DM Sans for UI. Don't mix.
- **Letter-spacing:** Cinzel labels use `0.25em` to `0.4em` letter-spacing consistently.
- **Numbers:** Use `font-variant-numeric: tabular-nums` on any counter / stat / duration / timestamp.
- **Empty states:** Every list has an empty state with a ♦ symbol and italic Cormorant Garamond messaging.
- **Toasts:** Confirmations use a bottom-center toast with a gold border, green check icon, and italic Cormorant.
- **Animations:** Use `cubic-bezier(0.25, 0.1, 0.25, 1)` for most entrance easings. Fade-in-up is the default entrance pattern.

---

## Quick tests to run after any change

```sh
# Local smoke test:
open index.html
```

Then manually verify:
1. Landing → click "Place Your Bet" → RSVP appears
2. Go through RSVP flow — should land on dashboard with welcome animation
3. Dashboard nav → Guests, Matches, Playlist, Album all navigate (desktop nav and mobile hamburger drawer)
4. Open a guest profile in The Hall → react → see confirmation toast
5. Matches page shows Adan + Liam pre-seeded
6. Playlist → search "burna" → submit → check "My Submissions" tab
7. Album → click a photo → lightbox opens with prev/next
8. Refresh the page — state persists

If any of these break, trace the console for errors first.

---

## Contact / ownership

- **Product owner:** Camara Talburt — all scope decisions route through him
- **Agency:** Assonance Ltd (London, 15852824)
- **Producer brand:** ProdByCamz
