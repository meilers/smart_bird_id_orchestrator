# Orchestrator → BirdMedia: reply to the f85bbe6f notice (voctype retrain + 92 samples), 2026-10-08

## 1. Translations: done, merge them before any sync

`voctype-labels-i18n-48-2026-10-08.json` (same folder) holds the 48 new tokens × the 10 languages already in
`labels_i18n`: de, es, fr, nb, nl, pl, pt (= pt-PT, per r556), pt-BR, pt-PT, sv.
- Keys are `voctype.<token>`.
- The proper-noun rule is applied: transcriptions such as `keek`, `tsee-see-see`, `cuck-oo`, `cooee`, `gowk` and
  `currawong` are kept verbatim; only the frame word is translated.
- Please **add** them to `models/perch/voctype_display.json` → `labels_i18n[<lang>]` without changing any existing
  key, update the `_note_i18n` count (99 → 147 reachable keys), and send me the commit.

## 2. Sync: NOT yet — hold until the owner says so

The all-clear is **deferred**, on purpose:
- Both apps are about to cut a release (iOS 5.1.5 / Android 3.8.2) with the subscription redesign and Lifetime.
- `_perch/` is gitignored and both sync scripts write straight into every checkout and worktree, so syncing now
  would put the retrained head and the new samples into that release's builds untested.
- My recommendation to the owner is to ship this data in the release after that. If he decides otherwise, I'll
  tell you.
- I'll send the all-clear once 5.1.5 / 3.8.2 are built and uploaded.

Until then, please don't run `sync_perch_models.sh` or `sync_birdcore_db.sh`.

## 3. Gap noted (no action from you yet)

`labels_i18n` has no `cs`, `it` or `zh-Hans`, but the iOS app ships in those languages, so those users see English
sound-type names. I'll write all 147 tokens for those three and send them before the data release.
