# HANDOFF — state as of 2026-10-06

Written by the previous orchestrator (Claude, in Cowork) for whoever continues (e.g. Codex). Read `AGENTS.md`
first for the workflow and standing rules.

## Where things live

- Requests, owner specs, translations, proofs: `<app repo>/_orchestrator/` in each app repo (see AGENTS.md).
- Owner specs (both app repos, `_orchestrator/owner-specs/`): `carousel-banner-2-hoodie-giveaway`,
  `giveaway-screen-v2`, `carousel-start-banner-rotation`, `identify-widget-v2(-polish)`, `identify-button-haptics`,
  `copy-audit-list-prepositions`, `system-entry-points`, `journal-rebuild` (+ mockups),
  `journal-award-badge-and-empty-state`, `my-stickers-redesign`, `new-icon-and-splash`.
- Brand sources (both repos): `_orchestrator/brand/smartbirdid.png` (app icon, full mosaic square, 1254 px),
  `bird-head.png` (transparent bird mark, splash only), `bird-head-bounds.json`.
- Release notes: `smart_bird_id_ios/_orchestrator/release-notes/ios-5.1.4.md` (19 locales),
  `smart_bird_id_android/_orchestrator/release-notes/android-3.8.1.md` (en, es-ES, es-419, fr-FR, fr-CA).
- Shared parity fixtures (iOS writes, Android copies byte-identical with sha256 tests): sticker style
  (`_orchestrator/sticker_style/`), sample trim v2 (`_orchestrator/sample_trim_v2/`), empty-Journal geometry
  (`_orchestrator/journal-empty/geometry.json` v2 + `journal_empty_parity.json`), widget card background.

## Counters (next numbers to use)

| Thing | Last used | Next |
|---|---|---|
| iOS request | r647 | **r648** |
| Android request | r640 | **r641** |
| Cloud request | r616 (cancelled) | r617 |
| Translation batch | 27 | **28** |

## Releases

- **Submitted:** iOS **5.1.4 (724)** to the App Store; Android **3.8.1 (358)** to Google Play. Tags requested:
  `ios-5.1.4-724`, `android-3.8.1-358` (confirm they were pushed: `git tag -l`).
- `main` on both repos contains everything below; `feature/journal-rebuild` was merged (`--no-ff`) and kept.
- **Release-day to-dos for the owner:**
  - App Store Connect: **remove from sale** the 8 sticker-pack IAPs:
    `com.sobremesa.SmartBirdID.stickers_regional` / `.stickers_all`, and the same under `.europe.`, `.au.`, `.in.`.
    (Android never sold sticker packs.)
  - Firebase Remote Config: `visual_intelligence_enabled` stays **false** (optional condition for owner's phone);
    check `show_contest` is **on for iOS** (it read off on simulators once).

## What shipped in this release (both platforms unless noted)

- **Identify redesign:** carousel (Bird of the Day, monthly hoodie giveaway, "New widget" card #3 with the live
  widget over background C), carousel start banner rotates per app session (cold start or ≥30 min background;
  persisted `carousel.lastStartBannerID`), sticker-style pastel circles, Life List check, haptics.
- **Giveaway screen v2** (one screen for carousel + post-sighting; "entered" = email actually sent;
  `sb_enter_contest {source}` on both).
- **Identify widget v2** (+ iOS Lock Screen widget and Controls "Sound ID"/"Photo ID").
- **System entry points:** iOS Home Screen quick actions, App Shortcuts/Siri (19-locale phrases incl. "Open/Start
  Sound ID in Smart Bird ID"), Action button/Lock Screen controls, Spotlight species index (BirdEntity, action
  subtitle, hidden offline keywords), `.system.search/.open` schemas, Visual Intelligence (cloud pipeline first,
  **behind RC flag, default off**). Android: launcher shortcuts (Photo ID, Sound ID, Search, Share), Quick
  Settings "Sound ID" tile. Gemini/App Actions skipped (AppFunctions private preview). Event `sb_app_shortcut`.
- **My Bird Journal rebuild** (new UI on existing domain; batched queries, downsampled thumbnails, no players in
  rows, square 88 thumbnails with count strip **on** the image, corner `star.circle.fill` award badge, native
  search on iOS, TODAY/YESTERDAY sections, empty state = notebook artwork + daily regional bird via the BOTD
  picker salt "journal-empty" with per-line text rotation).
- **Global app-icon menu** on all 5 tabs (incl. free users' Store tab): View My Stickers (`bird.circle`), Add the
  Identify widget | Share Smart Bird ID, Send feedback | version footer (custom compact popover on iOS, custom
  popup on Android). iOS Profile notifications bell + screen **deleted** (owner: hide). Android Store had no
  share icon.
- **Stickers:** Sticker Packs IAP removed; **earned-only for everyone (subscribers too)** — adding a bird to your
  list unlocks its sticker; past pack buyers keep their unlocks (grandfathered). My Stickers redesigned:
  full-screen modal, X | My Stickers | Share, Collected | All, search kept across scopes, A–Z grid, uncollected
  look in All. Animations: paywalled on iOS, **free on Android** (owner: keep).
- **New app icon** (mosaic) everywhere in-app; **new splash**: black in both modes, bird mark + "Smart Bird ID".
  iOS launch storyboard (34 pt semibold, +14 pt gap, lockup at 48 %). Android uses SplashScreen API candidate
  **B** (lockup inside the icon area; bird smaller than iOS) — one-line switch to **A** (bird centred, wordmark
  as bottom branding image) if the owner prefers. Android launcher icon = full square on the 72 dp viewport.
- **Fixes:** iOS Kingfisher memory limit actually applied (r635); Android Journal no longer overwrites sighting
  times with midnight (r628) + one-time non-destructive repair for build 258 users (r637, `sb_repair_midnight`);
  Android carousel swipe fix (NestedScrollableHost).

## Open items / decisions pending

0. **Active (2026-10-07): subscription screen redesign + free backup/stickers** — owner spec
   `owner-specs/subscription-screen-redesign.md` (+ two mockup .webp) in both app repos. Requests iOS r646 / Android
   r640 (branch `feature/subscription-redesign`), batch 26. Free cloud backup PARKED by the owner (much larger
   backup scope planned; gates untouched). Audit notes: free users lack a real Firebase identity, iOS `createUser`
   `user.reset` can wipe local sightings, sync also writes `sharedSightings` when toShare. Android: no Family product, carousel retired, Restore/Redeem/Important Info added. Pending owner:
   Android 1-sighting/24h free cap (`canSubmitIdentification`).
   2026-10-07 reports: iOS db4eb7872e (unpushed branch), Android e50cb846e+f27201d81 (pushed). iOS r647 = fixes
   (spinner overlap, icon balance, close X, dismiss after purchase, restore feedback, hasProFamily check, typo).
   Android r641 waits on owner: dark vs light-only, hero photo, 24h cap, MEMBER50 Store-tab offer (both platforms).
1. **Three older iOS glitches** (PARKED on the roadmap by the owner 2026-10-07; strings purge #8 parked with it): after saving a sighting the "Sharing is
   caring" prompt opens with the share sheet on top (two dismissals); the saved card's photo doesn't fill it; the
   "Set as avatar" tip covers the sticker title on first open.
2. **Android splash A vs B** — owner to look on his Pixel. (Pixel left in Light mode by agents; owner may want Dark.)
3. **Manual sightings and shared data** — AUDITED 2026-10-07, **PARKED on the roadmap by the owner**. Findings:
   no-media sightings of Pro users sync to `users/{uid}/sightings` and, if `toShare`, to public `sharedSightings`
   (Feed/Nearby/map/profiles) and fire `sendBirdAlerts` (cloud `na/functions/index.js:613`, ignores `hideAddress`).
   iOS defaults share OFF without media (`SubmitSightingViewController.swift:66-83`); Android defaults share ON for
   Pro + regional bird regardless of media (`SubmitFragment.kt:894, 1290`). Android `SubmitActivity.kt:82-91` may
   tag a manual add with a stale `IdResultManager` result. Firestore/Storage rules are not in any repo.
   Proposed fix (not approved): no-media → private only on both; alerts only for media + skip `hideAddress`.
4. **Android navigation modernisation** (Material 3 / Expressive: surface top bar edge-to-edge, flexible nav bar
   with labels) — proposed as its own round after this release; not started.
5. **Next data release** (BirdMedia sync **hold** still on): three retrains, `labels_i18n` (now in BirdMedia,
   78ba49a5), plain example-media rows, translations for ~100 new voctype tokens; 107 NA birdie drawings differ
   between iOS and Android (owner's sticker batch — commit `Birdies_*.zip` when final).
6. **"How to identify birds"** screen — menu item exists behind a flag; design pending from the owner.
7. **Android AD_ID declaration** — recommended keep; owner to confirm in Play Console.
8. iOS unused strings to purge in a strings pass: `stickers.pack.*`, `stickers.packs.*`, `product.stickers_*`,
   `stickers.email.backyard.*`, `profile.alert.notifications.*`.
9. Android r622 note: the widget on card #3 follows light/dark like the real widget (owner accepted).

## Facts that are easy to get wrong

- Bird list = bundled `smartbirdid.sqlite3`; Perch labels map via the `ebird_code` column; BirdCore is a resolver.
- Location drives species filtering, not the continent flag columns.
- Android has **high-res photos** via on-demand `highResImages` modules (installed by `MainActivity`).
- The widget's "+" = the classic Identify toolbar search (`openSearchByName`), not a species picker.
- iOS caches launch screens: to see a new one, delete the app **and restart the phone**.
- Android agent can't read `~/Downloads` (macOS privacy) — have the owner copy files into the repo.
- Simulators can't test Siri, Visual Intelligence, the Action button press, hitches, or Core ML accuracy
  (iOS 27 simulator runs these classifiers degenerately) — ask the owner for device checks.

## The full previous transcript

The Cowork session transcript (very long) is at
`/var/folders/_j/gyyctb2x6jg0npql0dy168v80000gn/T/claude-hostloop-plugins/89910d6c51e557ca/projects/session/fc35f414-bd27-4bab-8e10-75888527f82f.jsonl`
on the owner's Mac (temporary folder — copy it here if you want to keep it).
