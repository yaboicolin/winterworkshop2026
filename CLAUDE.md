# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repo contains static HTML materials for the AI Winter Workshop — a facilitated 2-hour pair-programming session teaching AI tools (Microsoft Copilot, vibe-coding) to non-technical practitioners (program managers, change leads). There is no build step, no package manager, and no test suite.

## Development

Open `winterworkshop.html` directly in a browser. No server needed — all assets are local (images referenced by filename). To preview changes, refresh the browser.

## Architecture: winterworkshop.html

Everything lives in one file: CSS (`:root` CSS variables for theming), HTML (sections), and JavaScript (inline `<script>` at the bottom).

**Navigation model:** Workshop sections (`<section id="...">`) are all present in the DOM and toggled with `.hidden`. `showSection(id)` unhides the target and marks the matching sidebar `<nav>` button active. Navigation is triggered via sidebar buttons (`data-target`) or in-section "Next" buttons (`data-nav-next`).

**Timer system:**
- Global timer (120 min total): `startGlobal` / `pauseGlobal` / `resetGlobal` control a `setInterval` tick; displayed in the topbar chip.
- Per-section timers: `initSectionTimer(el)` reads `data-duration` (seconds) from the section element and wires up `.sectionStart/.sectionPause/.sectionReset` buttons. Inspiration and Break sections have visible section timers.
- Keyboard shortcuts: `g` toggles global start/pause, `r` resets global timer.

**Copy button system (two modes):**
- `[data-copy]` — button sits inside a `.prompt` container; copies the sibling `<pre>`'s text.
- `[data-copy-target="#id"]` — copies text from the element matching the CSS selector.

**Theme system:** Dark by default. CSS variables are defined in `:root` (dark) and overridden by `[data-theme="light"]` and `@media (prefers-color-scheme: light)`. User preference is persisted to `localStorage` under the key `"theme"`.

**Image loading:** `governance_example.jpg` and `iphone_shot_1..4.jpg` are lazy-loaded via JS (`img.onload` / `img.onerror`) so missing images fail silently rather than showing broken-image icons.

## Sections (workshop flow)

| `id` | Duration | Purpose |
|---|---|---|
| `welcome` | 5 min | Rules, pair roles |
| `prepare-copilot` | 5 min | Custom instructions for Copilot |
| `firestarter` | 5 min | Three quick AI exercises with copy-ready prompts |
| `copilot` | 25 min | Excel FTE dashboard challenge using Copilot |
| `break` | 5 min | Rest |
| `inspiration` | 25 min | Colleague showcases |
| `vibecode` | — | Vibe-coding demo + prompt ideas |
| `reflection` | 15 min | Wrap-up |

## Known Issues

### GitHub Push Authentication
Pushing to GitHub from this machine requires authentication. Neither SSH keys nor the GitHub CLI (`gh`) are configured. HTTPS pushes fail without credentials.

**Workaround:** Generate a Personal Access Token at github.com/settings/tokens with `repo` scope ticked, then run:
```
cd ~/Documents/winterworkshop2026 && git push origin main
```
Enter your GitHub username and paste the token as the password when prompted.

**Note:** Do not embed the token in the remote URL (`git remote set-url`) — remove it after use with:
```
git remote set-url origin https://github.com/yaboicolin/winterworkshop2026.git
```

**Status: Resolved.** `gh` is installed and configured as the git credential helper (`gh auth setup-git`). Pushes work without manual tokens.

## Session Log

### 2026-05-23
- Connected Claude Code to GitHub via MCP
- Identified 2 modified files (`CLAUDE.md`, `winterworkshop.html`) and 6 untracked screenshots from the 22 May session
- Committed and pushed all local changes to `origin/main` (2 commits total, including the prior unpushed KPMG redesign)
- Resolved GitHub push auth by installing `gh` (`brew install gh`) and completing device login at github.com/login/device
- Configured `gh` as the git credential helper via `gh auth setup-git`
- Added "Showcase 3 — Coen: HTML-Mogged by Claude Design" callout to `winterworkshop.html` Inspiration section
- Extended Inspiration section timer from 25 min to 35 min (heading, chip, `data-duration`, sidebar badge)
- Fixed welcome heading typo: "Winterschool" → "Winter Workshop"
- Confirmed system timezone is correctly set to `Australia/Sydney` (AEST)
- Committed and pushed all changes to `origin/main` (commit `4e7484d`), verified via GitHub MCP

### 2026-05-26
- Pulled latest `winterworkshop.html` from GitHub via MCP (GitHub version was 14 bytes ahead of local)
- Added 5-min countdown timer (Start/Pause/Reset controls + clock chip) to the Micro-break section, positioned below "Refresh and reset" with spacing above the Next button
- Updated CLAUDE.md: corrected file references from `index5.html` → `winterworkshop.html`, updated timer system note, resolved GitHub auth known issue
