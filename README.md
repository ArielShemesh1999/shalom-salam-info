# שלום / سلام — Shalom-Salam

> **A bidirectional Hebrew↔Arabic learning app for phones — one static HTML file, fully offline, no backend, no tracking.**

**There is no live deployment** — not hosted on the web, not in the App Store or Google Play. It runs as an installable PWA and as native iOS/Android builds from Capacitor 6 (app ID `com.arielshemesh.shalomsalam`), and was installed and running on the author's iPhone as of 2026-05-14. Submission is still gated on a hosted privacy policy and store listing assets. Source is private; this repo is the description.

## What the app teaches

Five stages — letters → numbers → words → roots → sentences — with timed rerun modes (roots and sentences each run as one regular block plus one timed rerun), on a winding lesson-node map, not a flat list. On top: four exercise types (translate, listen-and-choose, tap-to-match, word-bank sentence build), a mistakes bucket for spaced review, a daily-XP goal ring, streaks with earned freezes, a simulated weekly league.

## Why it is one HTML file with no bundler

The app is a single ~17,000-line `index.html` (~762 KB) plus two vendored libraries, GSAP and Motion. No framework, no bundler: `npm run build` copies web assets into `www/` for `cap sync` and strips `console.log` from production. Runtime dependencies are Capacitor plugins only — a language app you use on a plane has nothing to fetch and nothing to resolve.

## How offline was made non-negotiable

Fonts are self-hosted and subsetted per script — Heebo for Hebrew, Noto Sans Arabic for Arabic, twenty `.woff2` files, no CDN. The service worker (`ss-v5`) precaches the shell plus the exact weights first render needs; pronunciation is spoken on-device through the Web Speech API, so there is no speech service to call. Progress lives in browser storage and Capacitor `Preferences`. No external services at all.

## What two right-to-left scripts cost

Hebrew and Arabic share one interface, so direction is a per-element property, not a page setting: `unicode-bidi: isolate` on mixed-direction cards, explicit `lang="ar"` so a screen reader does not read Arabic in a Hebrew voice. Accessibility was retrofitted ARIA-only onto the finished design — `aria-checked` radio semantics, focus moved on navigation, AA contrast, 44 px tap targets, pinch-zoom restored.

## What was cut to keep an old iPhone smooth

A 245 KB `moveable.min.js` was replaced by a ~30-line native Pointer Events helper carrying both gestures; a 107 KB PNG favicon became a 7.5 KB icon. Net shipped payload fell ~352 KB (vendor 449 KB → 204 KB). Idle mascot tweens run only on the visible page, not forever across all 27 pages.

## What store review forced

A fake ₪0.99–49.90 hint paywall was removed and hints made free — charging without real StoreKit/Play Billing is an Apple 3.1.1 auto-reject. Then in-app account deletion (5.1.1(v)), a `PrivacyInfo.xcprivacy` declaring no tracking, `targetSdk 35`, iPhone-only device family. Taught content was neutralised in the same pass: politically asymmetric example sentences rewritten to neutral settings, the owner's name removed from lesson data.

## How it was verified

`npm test` runs nine files under `node --test`: HTTP smoke, JSDOM structure over every stage's teach/quiz/fail page, native-config parity (Capacitor config, `Info.plist` URL scheme, `PrivacyInfo.xcprivacy`, Android manifest), and regex invariants pinning every prior bug fix against regression. End-to-end runs on Playwright 1.60.0 Chromium at iPhone SE (320×568), iPhone 12 (390×844) and desktop (1280×900): zero console errors on cold load, every `<section.page>` cycled without a JS error. The suite grew 36 → 72 tests as sweeps landed.

## Stack

`HTML` `CSS` `JavaScript` (no framework, no bundler) · `Capacitor 6` (six plugins) · `GSAP` · `Motion` · `Web Speech API` · Service Worker + Web App Manifest · `node --test` + `jsdom` + `playwright`

Built by [@ArielShemesh1999](https://github.com/ArielShemesh1999).
