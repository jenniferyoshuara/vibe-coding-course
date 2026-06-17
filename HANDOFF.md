# Project handoff

A self-contained context note so anyone (including a fresh Claude Code session on a new account) can pick this project up cold.

## What this is
An interactive, **single-file** web course — [`index.html`](index.html) — that teaches a non-programmer how to safely **operate and supervise AI-built coding projects**. It's anchored to a real use case: running a stock-research bot (analyzes tickers → JSON → Markdown articles). No build step is needed to view it; just open `index.html` in a browser.

## Where it lives
- **Local working copy:** `C:\Users\JenniferYoshurara\Desktop\Personal\vibe-coding-course`
- **GitHub (public):** https://github.com/jenniferyoshuara/vibe-coding-course
- **Live site (GitHub Pages):** https://jenniferyoshuara.github.io/vibe-coding-course/ — auto-redeploys on every `git push` to `main`.

## How `index.html` is structured
All content is JavaScript data inside the single `<script>` block:
- `syllabus` — 8 weeks. Each has: `title`, `focus`, `difficulty`, `time`, `topics`, `lesson` (an HTML string — the chapter, opening with an inline SVG diagram), `learn`, `tasks`, `commands`, `deliverable`, and `quiz` (6–7 questions). Some weeks also have a `builder` activity.
- `projects` — 5 applied projects, each with its own `quiz`.
- `references` — cheat-sheet tables.
- Learner progress (checkboxes + quiz answers) is saved in the browser via `localStorage`.

Each week's lesson is deep (~1,100–1,900 words), beginner-friendly, with **worked walkthroughs** (command → output → meaning) and a themed inline-SVG **diagram** at the top. The diagrams use the page's CSS variables, so they match the theme automatically.

## How to make edits

### Small edits
Just edit the relevant string/array directly in `index.html` and `git push`. (Tip: each week's `lesson` is one long JSON-style string; the quizzes are JSON arrays.)

### Large content regenerations (the diagram/lesson pipeline)
Source fragments live in a **git-ignored** `.build/` folder (so they are NOT on GitHub — only the assembled `index.html` is committed). The flow:
1. Per-week fragments: `.build/week<N>.lesson.html`, `.build/week<N>.quiz.json`, `.build/week<N>.diagram.svg`, plus `.build/hero-arc.svg`.
2. Assemble: `node .build/build.js` — it scans `index.html`, replaces each week's `lesson:` literal and `quiz:` array using `JSON.stringify` (and escapes `</` → `<\/` so nothing breaks the inline `<script>`), and injects the diagram CSS + hero arc.
3. Validate: parse the `<script>` body with `new Function(...)` to confirm no syntax errors before committing.

> ⚠️ Because `.build/` is gitignored, regenerating from scratch on a fresh clone requires re-creating those fragments first. The committed `index.html` is the source of truth for the live site.

## Local preview
`.claude/launch.json` (in the main Claude Code working dir, not this repo) serves the repo with `python -m http.server 8753`. Or just double-click `index.html`.

## Git / GitHub notes
- Authenticated as **jenniferyoshuara**; commits use her GitHub no-reply email so they link to the account.
- The `gh` CLI is installed but **not on PATH**; call it by full path:
  `C:\Users\JenniferYoshurara\AppData\Local\Microsoft\WinGet\Packages\GitHub.cli_Microsoft.Winget.Source_8wekyb3d8bbwe\bin\gh.exe`
- Switching Claude accounts does NOT affect this project — the folder, git history, and `gh` login are all stored on the machine, independent of Claude.

## Status & possible next steps
- ✅ Done: 8 deep lessons, 8 per-week SVG diagrams, hero learning-path arc, expanded quizzes (weeks + projects), progress tracking, cheat sheets, GitHub repo + live Pages.
- 💡 Optional / not yet done: an "operator loop" global diagram (*Ask AI → Read diff → Run tests → Commit/Reject*); real screenshots (PowerShell, a `pytest` run, Windows Task Scheduler); a final end-of-course "operator exam" pulling questions from all 8 weeks.
