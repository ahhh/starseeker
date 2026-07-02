# Star Seeker

A single-page field guide to tonight's sky. Enter a date and a location, and Star Seeker tells you which constellations are visible, when and where to look for them, how to spot them, and a bit of the mythology behind each one.

Everything runs in the browser — no backend, no build step, no API keys. It's one `index.html` you can host anywhere, including GitHub Pages.

## What it does

- **Answers "what can I see tonight?"** — pick a date and location and get the constellations that climb above 20° altitude during true darkness, sorted by how high they rise.
- **Tells you where and when to look** — each result shows the best viewing time, compass direction, altitude, and an easy / moderate / hard difficulty rating.
- **Shows you the pattern** — click any constellation to open an "atlas plate": the stick figure drawn from real star coordinates, with guide stars labeled, plus a plain-language spotting guide and short lore.
- **Reads the conditions** — reports the night's window of true darkness and the moon phase, and warns when a bright moon will wash out fainter stars.

## Finding your location

- **Type a place name** — "Denver", "Tokyo", "Reykjavík" — and coordinates are looked up for you via the free [Open-Meteo geocoding API](https://open-meteo.com/en/docs/geocoding-api).
- **Use my location** — one click grabs your coordinates from the browser (with your permission).
- **Enter latitude and longitude directly** if you'd rather.

Your last location is remembered between visits.

## How it works

The astronomy is computed in plain JavaScript — no external libraries:

- Local sidereal time → hour angle → altitude/azimuth for each constellation's guide stars.
- A low-precision solar position model to find when the sky is actually dark (sun below −9°).
- A synodic moon-phase approximation for the illumination warning.
- Constellation figures are rendered as SVG on the fly from J2000 star positions and magnitudes — 18 constellations, ~140 stars.

All calculations happen locally; the only network request is the optional place-name lookup.

## Running it

Open `index.html` in any modern browser, or serve the folder:

```
python3 -m http.server
```

Then visit `http://localhost:8000`.

## Deploying to GitHub Pages

Push to `main`, then in the repo go to **Settings → Pages** and deploy from the `main` branch root. The site will be live at `https://<username>.github.io/starseeker/`.

## License

MIT — see [LICENSE](LICENSE)
