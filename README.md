# Meeting Timer

**Live: [m0jimo.github.io/meeting-timer](https://m0jimo.github.io/meeting-timer/)**

![Meeting Timer](docs/meeting-timer.png)

A retro DOS/TUI-styled meeting timer for tracking speaker time across team sessions.

## Features

- Track multiple teams and team members
- Record meeting sessions by date
- Time individual speakers with start/stop controls
- View per-member stats (total talk time, turns, average)
- Keyboard navigation between panels
- Norton Commander / DOS aesthetic

## Usage

Open `src/index.html` directly in a browser — no build step, no dependencies. Reload page after change.

## Layout

```
┌─ Teams ──┬─ Sessions ──┬─ Recording Zone ─────────────────┐
│          │             │ (visible when a session is active) │
├─ Members ┴─────────────┴─ Stats ───────────────────────────┤
│          (always visible)                                   │
└─────────────────────────────────────────────────────────────┘
```

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Tab` | Cycle panel focus (Teams → Dates → Members) |
| `↑` / `↓` | Navigate items in focused panel |
| `Enter` | Select item |
| `N` | New team / new date |
| `Space` | Start/pause session timer |
| `Delete` | Delete selected item |

## Data

Persisted to `localStorage` under the key `meeting-timer-v3`.

```
teams:    [{ id, name, members: [{ id, name }] }]
sessions: { teamId: { dateStr: [{ memberId, totalMs, turns }] } }
```
