# Diary UI QA (1.3.9)

The 2026-10-13 local import build redesigns Personal Journal and My Diary using
light-blue accents, visible date/text previews and an eight-item directory.
Search checks the complete stored body, including text beyond the displayed
excerpt. Directory filters and pagination survive a reader roundtrip and reset
when switching characters. Reading another page starts at the top; continuation
of the same page does not reset the scroll position.

Personal Journal now supports an optional page title: a normalized single line
of at most 40 characters. New pages can receive their own title; continuation
preserves an existing title and may fill an untitled legacy page. The directory
shows the title in place of the generic character-journal label, and the reader
uses it as the main heading. Title search, archive retention and legacy fallback
are covered without rewriting production diaries.

Personal Journal no longer shows write counts or a reader position/total fraction.
The reader heading and sticky footer use the persisted diary number; the footer
also shows the current title, falling back to the date range for untitled diaries.
Previous/next diary buttons select another complete diary. First-entry and
continuation labels identify the writes inside one diary, each with the full
stored date and time. Catalog pagination explicitly says "List".

My Diary now receives an optional independent title in the same model request
as the body. Structured replies, legacy plain-text replies, title/body limits,
rejected incomplete replies, persistence rollback, archiving and restart are
covered. Untitled legacy entries retain their generic heading. Search covers
titles as well as dates and the complete body.

My Diary navigation selects a complete entry. Its heading and footer identify
the entry by title and full stored composition date/time, without a position/total
fraction or paragraph count. Interaction counts appear only in the expandable
reference material and the catalog tooltip, with an explicit label. Both
directories identify their pagination as "List". The last composition timestamp
also triggers refresh when a full shelf replaces an old entry without changing
its count.

The final isolated verification passed:

- Hosted-supported TSX typecheck and linker: 31 dependencies / about 462 KiB,
  within the host's 32 dependency / 512 KiB limits.
- `../dist/qa-diary-1313-review-titles-final/report.json`: 94 interaction checks and
  24 screenshots; no check, runtime or layout failures, including
  41 compact/footer regressions across all eight locales, desktop/narrow
  widths, short/tall windows and light/dark themes.
- Complete Python suite: 853 passed, 38 explicit environment skips.
- Focused review/title/archive/persistence/host-pipeline tests: 119 passed.
- Ruff's CI-compatible checks and official isolated package validation passed.

The new workflow suite covers directory pagination, full-text filtering,
no-result recovery, source text preservation, navigation boundaries, reference
expansion, archive failure/retry, archive-only directories, empty diaries and
legacy entries without snapshot fields. Signature and navigation text stay
inside the available bounds; sticky navigation clears the measured save bar.
Surviving diary numbers start at 53 in the many-diary fixture, proving that reader
identity does not come from catalog position. Tests cover same-diary continuation
markers, complete diary switches, archived identity, same-row navigation in all
eight locales, and live continuation preserving both identity and scroll position.
My Diary checks verify the footer title and timestamp change together with
the body in current and archived entries, interaction counts stay hidden until
reference expansion, and legacy entries do not acquire invented zero counts.
Title-only search and automatic title refresh with an unchanged shelf count
are covered. Review navigation also stays on one row across all eight locales.

```powershell
node tools/hosted_ui_harness.mjs --diary-only --compact --typecheck --out ../dist/qa-diary-1313-review-titles-final
```

Screenshots contain deterministic fixture data. Visual verification uses the
isolated Chromium harness captures; the harness closes its test browser and
temporary server when it finishes. No production browser session is needed.

The package was built using `tools/build_import_package.py` against the read-only
host SDK. Archive CRC, manifest version, payload hash and packaged source bytes
were verified. No host source, Steam installation or production Store was
modified. Steam import is the author's acceptance step. The author requested
version 1.3.9 and a repository commit/push for this diary update. Distribution
is limited to that update and the local import package; no release tag or
GitHub Release is created.
