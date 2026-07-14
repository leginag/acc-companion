# ACC Companion

A phone-installable web app for preparing the **ICF ACC credential**: the full programme **learning materials** rendered natively with per-module summaries, practise drills built on the programme's six-phase coaching conversation, the 8 ICF Core Competencies with the official ACC assessment criteria, a 60-question mock **ACC Exam** simulator, and a **100-hour experience log** that enforces ICF's counting rules (75 paid / 8 clients / 25 hours in the 18 months before applying).

- **Live app**: https://leginag.github.io/acc-companion/ — opening the app requires an access code (held by the owner). The programme materials in `learn-data.js` are stored AES-256-GCM-encrypted; without the code they are unreadable, including in this repository.
- All data stays in the browser's localStorage on your device — client names never leave the phone. Use *Hours → Download backup* to move devices.
- ICF rules encoded as of **2026-07-14**, verified against the ACC Candidate Guide (rev. March 2026) and coachingfederation.org (see the in-app Runway tab for the policy watch-list). Not affiliated with ICF; exam practice items are original and unofficial.

## Tabs

Learn (module summaries + verbatim materials) · Practise (six-phase question bank, flashcards, spaced-repetition language drills, full-conversation rehearsal) · Competencies (8 ICF Core Competencies + ACC behavioural statements) · Exam (timed mock + drills + verified exam facts) · Hours (100-hour log + mentor coaching) · Runway (application checklist + policy watch).

## Development

App code lives in `index.html`; the Learn tab's content lives in `learn-data.js`, generated from the programme's workbook PDFs and handouts (do not edit by hand — regenerate from the source materials). Preview locally with `powershell -File serve.ps1` → http://localhost:8321/ (dependency-free static server). Deploys to GitHub Pages automatically on push to `main` via `.github/workflows/deploy.yml`.

Append `?fast=1` to the URL to shrink mock-exam timers ×60 for testing.
