# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file vanilla JS app — no build step, no dependencies, no package manager. Open `src/index.html` directly in a browser.

## Architecture

Everything lives in `src/index.html`:

- **CSS** (lines ~10–912): DOS/TUI theme (Norton Commander aesthetic). CSS variables in `:root`. Layout is a full-viewport flex column: titlebar → toolbar → content → statusbar.
- **HTML structure** (lines ~914–1019):
  - `.main-top`: flex row with Teams panel | Sessions panel | Recording zone (recording shown only when a date is selected)
  - `.main-bottom`: flex row with Members panel | Stats panel (always visible)
  - Recording zone and Members panel both get a yellow outline (`panel-focused`) when a session date is active
- **JS** (lines ~1095–1885): all inline, no modules
- no single quotes

## Key layout rules

The full flex chain must have `min-height: 0` at every level (`body` uses `height: 100vh`, `.window`, `.content`, `.page`, `.main-top`, `.main-bottom`, panel children) to prevent content in one panel from expanding the row height.

## Data model

```js
db.teams   = [{ id, name, members: [{ id, name }] }]
db.sessions = { [teamId]: { [dateStr]: [{ memberId, totalMs, turns, lastStart, lastTalkTime }] } }
```

Persisted to `localStorage` under key `meeting-timer-v3`.

## State variables

| Variable | Purpose |
|---|---|
| `selectedTeamId` | Currently selected team |
| `selectedMemberId` | Currently selected member (for stats) |
| `selectedDate` | Active recording session date; `null` = browse mode |
| `sessionSpeakerId` | Member currently speaking |
| `focusedPanel` | `'teams'` \| `'dates'` \| `'members'` |
| `teamCursor / dateCursor / memberCursor` | Keyboard navigation position per panel |

## Render flow

- `renderMainPage()` — calls all sub-renders
- `renderMembersPanel()` — shows/hides recording zone based on `selectedDate`; also toggles `panel-focused` on the members panel
- `highlightFocusedPanel()` — applies `panel-focused` outline to the active keyboard panel
- `tick()` — runs every 500ms; updates clock and re-renders recording table if a speaker is active

## Panel focus & yellow frames

- `panel-focused` CSS class: `outline: 2px solid var(--accent-yellow)`
- Applied via `highlightFocusedPanel()` based on `focusedPanel` state
- Recording zone gets `border: 2px solid var(--accent-yellow)` via `.recording-zone:not(.hidden)` CSS
- Members panel gets `panel-focused` whenever `selectedDate` is set (recording mode active)
