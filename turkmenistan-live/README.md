# Turkmenistan — Living Isochronic Chart

A self-contained, real-time travel-time map of Turkmenistan, styled after Francis
Galton's 1881 Isochronic Passage Chart. One file. No build step, no API keys, no backend.

`index.html` is the entire application.

## What's real vs. simulated

**Real (live):**
- **Ashgabat local time** — UTC+5, no DST, via the browser's `Intl` time-zone engine.
- **Weather** for all 44 cities — fetched from [Open-Meteo](https://open-meteo.com)
  (free, keyless, CORS-enabled) on load and every 10 minutes.
- **Sunrise / sunset** — drives the day → dusk → night skin over the map.

**Simulated — but driven by the real clock and real weather:**
- **Road congestion** — morning/evening rush curves, weekend easing, and
  per-segment susceptibility (city approaches choke; desert highways barely move).
- **Road incidents** — accidents / road works / heavy traffic, deterministic per 4-minute window.
- **Domestic flight board** — a Türkmenhowaýollary-style schedule whose statuses
  (check-in / boarding / in-air / delayed / cancelled) come from the live clock and
  airport weather. Air routing must wait for the next catchable departure, so flying
  is faster at some hours than others — and unavailable late at night.

All of it feeds a Dijkstra solver over a multimodal network (53 road links · 16 rail
segments · 10 flight legs), so the isochrone bands genuinely shift through the day.

**Offline fallback:** if the network is blocked, it generates a deterministic, plausible
weather model instead and flips the badge to `SIMULATION`. It always works.

## Deploy

It's a static file — drop it on any host.

- **GitHub Pages:** commit `index.html` to a repo, enable Pages on the branch. Done.
- **Netlify / Vercel:** drag the folder onto the dashboard, or point a project at the repo.
  No build command, no output dir config needed.
- **Any web server / S3 / local:** just serve the file over HTTP(S).

> Open-Meteo is called over HTTPS, so the page works on any `https://` host with no setup.
> Opening `index.html` directly from disk (`file://`) may block the weather fetch in some
> browsers — that just triggers the offline simulation; everything else still runs.

## Controls

- Search or click the list to pick an origin; or click anywhere on the map for the nearest city.
- Toggle Car/Bus, Rail, Air, road Network overlay, ring Labels.
- Keyboard: `/` focuses search; `↑`/`↓`/`Enter` navigate the city list.
- Shareable origin via the URL hash, e.g. `…/index.html#o=17` (Daşoguz). Mode choices persist locally.

Not for ticketing, emergencies, border crossings, or operational travel.
