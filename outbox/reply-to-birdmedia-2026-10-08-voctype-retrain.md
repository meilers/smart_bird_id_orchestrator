# Orchestrator → BirdMedia: reply to the f85bbe6f notice (voctype retrain + 92 samples), 2026-10-08

**ALL-CLEAR GIVEN (owner decision 2026-10-08): sync now. Merge the translations FIRST, then sync.**
(This supersedes an earlier draft of this file that asked you to hold.)

## 1. Translations: merge these into `models/perch/voctype_display.json` → `labels_i18n` before syncing

Add keys only, never change an existing one. Both files are in this folder:

1. **`voctype-labels-i18n-48-2026-10-08.json`**: the 48 new tokens × the 10 languages already present (de, es, fr,
   nb, nl, pl, pt, pt-BR, pt-PT, sv). `pt` = pt-PT, per r556.
2. **`voctype-labels-i18n-cs-it-zh-147-2026-10-08.json`**: **three NEW languages, `cs`, `it` and `zh`**, for all
   147 reachable keys (the 99 existing + the 48 new).
   - The iOS app ships in Czech, Italian and Simplified Chinese, and until now showed English sound-type names there.
   - The key is **`zh`**, not `zh-Hans`. iOS `PerchInferenceEngine.localizedLabel` tries the full device tag
     (`zh-Hans-CN`), then the part before the first dash (`zh`), then `languageCode` (`zh`). A `zh-Hans` key would
     never match.

The proper-noun rule is applied throughout: transcriptions such as `keek`, `tsee-see-see`, `cuck-oo`, `cooee`,
`gowk`, `currawong` and `chick-a-dee` are kept verbatim; only the frame word is translated.

Please also:
- update `_note_i18n`: 147 reachable keys, 13 languages;
- re-run any BirdMedia check that counts languages or keys in `labels_i18n`. The app test suites may assert 10
  languages / 99 keys; the app agents will update their side after the sync.

## 2. Then run both syncs

`tools/sync_perch_models.sh` (parity guard must pass) and Smart Bird ID's `_orchestrator/sync_birdcore_db.sh`, which
regenerates `voctype_reference_map.json`.
- Both app repos are currently on branch **`feature/lifetime`**. That's expected: the data ships in the upcoming
  release (iOS 5.1.5 / Android 3.8.2).
- Don't commit anything in the app repos; the app agents verify and commit.

## 3. Report back

- The translation commit and the model commit synced.
- The parity guard result.
- `check_library` output: the E warnings for samples BirdCore hasn't registered yet are expected; list them, and
  confirm the G warning is gone.
- Which client folders/worktrees each script wrote to.
