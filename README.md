# MicDropMinds Dashboard

Live public dashboard for [@MicDropMinds](https://www.youtube.com/@MicDropMinds) clip analytics and posting schedule.

**Live site:** https://anem15.github.io/micdropminds-dashboard/

## What it shows
- YouTube views (auto-refreshed every ~30 minutes from public RSS / yt-dlp)
- X and Instagram views (from `data/manual.json`, edited on the box)
- Upcoming posts and daily routines (America/Denver)

## How it works
This is a **static GitHub Pages** site. The page reads `data/*.json` in the browser — no server and no tokens.

On the Grok Bot box, `/workspace/dashboard/live/start.sh` runs a background loop that:
1. Runs `fetch_youtube.py`
2. Copies `youtube.json`, `manual.json`, `history.json`, and a published `schedule.json` into this repo
3. Commits and pushes **only if the data changed**

Source of truth for scripts and editable files: `/workspace/dashboard/live/` on the box.

## Data files (public only)
| File | Source |
|------|--------|
| `data/youtube.json` | Public YouTube RSS + yt-dlp |
| `data/history.json` | YouTube total-views history for the chart |
| `data/manual.json` | X / Instagram (manual numbers) |
| `data/schedule.json` | Upcoming posts + routines (+ future queue items at publish time) |

No secrets, tokens, or credentials are committed here.
