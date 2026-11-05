# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Marketing site for El Co-working Space Dahab: a single static page, `index.html`, with all CSS in one `<style>` block and all JS in one IIFE `<script>` at the bottom. No build step, no dependencies, no tests. Images (`*.jpg`, `logo.png`) sit next to `index.html` and are referenced by bare filename. Hosted on GitHub Pages at `https://elcoworkingdahab-sketch.github.io/` (the `og:image`/`og:url` meta tags hard-code this domain).

## Running locally

Open `index.html` directly, or serve the folder so `fetch` behaves like production:

    python -m http.server 8000

## Architecture notes

- **Styling**: design tokens are CSS custom properties on `:root` (cream/sand/gold/sage palette, `--font` = Plus Jakarta Sans from Google Fonts). Light theme only. Breakpoints at 1000px, 860px, 560px; `prefers-reduced-motion` is respected. Use the existing `.pad` / `.wrap` helpers and `.btn-*` classes rather than new one-offs.
- **Icons**: an inline SVG sprite of `<symbol id="i-…">` near the top of `<body>`; use with `<svg><use href="#i-name"/></svg>`.
- **JS behaviours** (all in the bottom IIFE): mobile burger menu; "open now" badge computed in `Africa/Cairo` time (hours are Mon–Fri 8 AM, Sat–Sun 9 AM, until midnight; they appear in the hero badge, FAQ, Visit section and the JS check, so keep them in sync); EGP/USD price toggle reading `data-egp` / `data-usd` on `.amt` elements and persisting the choice in `localStorage` key `elco-cur`; gallery lightbox over `#gal button img` (keyboard + swipe).
- **Prices** live only in the `data-egp` / `data-usd` attributes of the pass cards in `#passes`; the text is rendered by JS. The meta description also mentions the cheapest price ("from 250 EGP").

## External services (no backend in this repo)

- **Events** (`EVENTS_URL` in the script): a Google Apps Script web app backed by a Google Sheet. `GET` returns `{events:[{date:"YYYY-MM-DD", time:"HH:MM", title, description, organizer}]}`; the event form `POST`s JSON as `text/plain` (avoids a CORS preflight) and expects `{ok:true}` or `{error:"…"}`. The `website` field is a honeypot. Requests are reviewed by the manager before appearing.
- **Meeting-room booking**: a separate Apps Script URL linked from several buttons (search for `script.google.com`).
- **Contact**: WhatsApp `wa.me/201063910070` is the main CTA and is repeated across the header, hero, pass cards, visit section, footer and sticky mobile bar — change all occurrences together. Same for Instagram/Facebook/Google Maps links.

The Apps Script source is not in this repo; changes to the request/response shape must be made there too.
