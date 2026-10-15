# MicDropMinds Live Dashboard

The code is in `/workspace/dashboard/live/`. The older files in `/workspace/dashboard/` are the Oct 13 prototype and are not used.

## Public URL (phone-friendly)
**https://anem15.github.io/micdropminds-dashboard/**

Static GitHub Pages site. Repo: `anem15/micdropminds-dashboard` (clone at `/workspace/dashboard/micdropminds-dashboard/`).

## Run / restart
    /workspace/dashboard/live/start.sh            # restarts local server (port 5190) + 30-min YouTube→Pages refresh loop
    TUNNEL=1 /workspace/dashboard/live/start.sh   # also tries a cloudflared quick tunnel (box egress often blocks it)
Local URL: `http://127.0.0.1:5190/?t=<token>`. The token is in `live/.token`. Every request without `?t=<token>` gets a 403.
Public site needs no token.
There's no cron on the box, so rerun start.sh after the box restarts.
Logs: `live/server.log`, `live/refresh.log`, `live/tunnel.log`.

## Data
- `data/youtube.json`: written by `fetch_youtube.py`, which runs every 30 min. It uses public sources only: the RSS feed (views) and `yt-dlp --flat-playlist` on /shorts. Subscribers come from the public channel page and show as a dash while YouTube hides the count. Run `python3 live/fetch_youtube.py` to refresh now.
- `data/history.json`: YouTube total views at each refresh, used for the chart.
- `data/manual.json`: X and Instagram. Set `followers`, and each post's `views` and `likes`. Leave a value `null` to show a dash. Set `updated` on a platform to an ISO time (e.g. `2026-10-15T12:00:00-06:00`). To add a post, append `{"id","title","url","views":null,"likes":null}` to `x.posts` or `instagram.reels`. Optional `youtube_studio.video_views` is a map of YouTube video id → Studio view count; the frontend prefers those over `youtube.json`. Set `youtube_studio.updated` when you edit it. Edit atomically: write a temp file and `mv` it over.
- `data/schedule.json`: upcoming posts (`when` is ISO with offset) and the routine times in MT. Future non-rejected entries in `/workspace/videos/queue.json` that have a `scheduled` time also show up as "Short (queue plan)" on the published site.
- `publish_pages.sh`: syncs public JSON into the GitHub Pages repo and pushes only when data changed.

The local page re-polls `/api/data` every 60 s. The public Pages site re-fetches `data/*.json` every 60 s.
