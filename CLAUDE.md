# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

ACC Companion — a single-page, phone-installable PWA for preparing the ICF ACC coaching credential (UNSSC Level 1 programme). No framework, no build step, no dependencies: plain ES5-style JavaScript, hand-rolled state and rendering.

## Run & deploy

- Local preview: `powershell -File serve.ps1` → http://localhost:8321/ (dependency-free PowerShell HttpListener; also registered as `acc-companion` in the root workspace `.claude/launch.json`).
- Deploy: push to `main` → GitHub Pages via `.github/workflows/deploy.yml` (~30 s). Live at https://coaching.wbr.io/.
- `?fast=1` query param shrinks mock-exam timers ×60 for testing.
- There are no tests or linters; verify by loading the app and exercising the changed tab (check the console for errors).

## Architecture

Two files matter:

- **`index.html`** (~4,000 lines) — everything: CSS, markup shell, all app data, and all JS in one inline script. Layout of the script, in order: constants/helpers → content data (`QB` question bank with `sections` of kind `phase`/`closure`/`situation`, `AVOID` Leitner cards, `CC` competencies, `EXAM_BANK`, `POLICY`) → storage (`seed()`/`migrate()`/`load()`, localStorage key `acc.v1`) → transient `view` state object → view functions (`vLearn*`, `vPractise*`, `vSkills*`, `vExam*`, `vHours`, `vRunway`, `vSheet`) returning HTML strings → `render()` (innerHTML swap keyed on `view.tab`) → `ACTIONS` map dispatched from a single delegated click listener on `[data-act]` → boot.
- **`learn-data.js`** — generated `var LEARN_ENC = {...}`: the programme's course materials (module summaries + verbatim document HTML), **encrypted** with AES-256-GCM under a PBKDF2-SHA256 key derived from the app's access code (never write the code into this repo — ask the owner). The app shows a lock screen on load; a correct code decrypts via WebCrypto, populates `LEARN`, and is remembered per device in localStorage `acc.unlock.v1`. **Never edit `learn-data.js` by hand.** Source materials live in `..\ICT Coaching Course Level 1\`; transcripts and builder scripts live in `..\ICT Coaching Course Level 1\_extracted\`. Regeneration pipeline: (1) transcribe new module documents into `modules\M12.md` etc. following the existing structure (`## Summary`, then `## <Doc title> {data-source="<filename>"}` per document, verbatim; `docx2md.py` converts .docx to markdown first); (2) `python build_learn.py <modules-dir> <tmp>\learn-data.plain.js`; (3) `python encrypt_learn.py <tmp>\learn-data.plain.js <repo>\learn-data.js <access-code>` — and delete the plain file. The app guards on `LEARN_OK` so it degrades if the file is missing.

Conventions that must hold:

- All persistent state changes go through mutating `db` then `save()`; `migrate()` must accept any historical shape (add a defaulted field in both `seed()` and `migrate()` when extending the schema).
- User content is escaped with `esc()` before interpolation; only trusted generated HTML (LEARN docs, question bank `qtext`) is inserted raw.
- Tab identity: the Competencies tab keeps internal id `skills` (`view.tab === 'skills'`, `view.comp`) for storage compatibility — only its label says Competencies.
- The six-phase model (Creating Context → Establishing Focus → Exploring Meaning → Deepening Insight → Generating Possibilities → Committing to Action) is the course's canonical map; "The Close" is a protocol (`kind: 'closure'`), not a seventh phase.
- Dark/light theming via CSS custom properties on `:root[data-theme]`; palette is CVD-safe (accent green `#1c8158`, blue `#2a78d6`) — do not regress.
- `sw.js` is network-first; bump the `CACHE` version string when shipping significant changes.

## Accuracy rules

ICF facts (exam format, application requirements, Code of Ethics, competency wording) are load-bearing: the exam currently tests the **2019 Core Competencies and 2020 Code of Ethics** (an update to the 2025 documents is pending). Verify any ICF claim against coachingfederation.org before changing it, and update `POLICY.asOf` plus the Runway policy-watch items when rules change. Exam items are original — never copy ICF's own exam/sample questions verbatim.
