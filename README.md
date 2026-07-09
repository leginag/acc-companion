# ACC Companion

A single-file, phone-installable web app for preparing the **ICF ACC credential**: practise drills from a coaching-programme question bank, the 8 ICF Core Competencies with the official ACC assessment criteria, a 60-question mock **ACC Exam** simulator, and a **100-hour experience log** that enforces ICF's counting rules (75 paid / 8 clients / 25 hours in the 18 months before applying).

- **Live app**: https://leginag.github.io/acc-companion/
- All data stays in the browser's localStorage on your device — client names never leave the phone. Use *Hours → Download backup* to move devices.
- ICF rules encoded as of **2026-07-08** (see the in-app Runway tab for sources and the policy watch-list). Not affiliated with ICF; exam practice items are original and unofficial.

## Development

Everything lives in `index.html`. Preview locally with `powershell -File serve.ps1` → http://localhost:8321/ (dependency-free static server; the machine needs no node/python). Deploys to GitHub Pages automatically on push to `main` via `.github/workflows/deploy.yml`.

Append `?fast=1` to the URL to shrink mock-exam timers ×60 for testing.
