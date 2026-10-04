# Perth Transit Boards

Platform-style departure signs for Perth trains and buses that run entirely in your browser.

Pick a train station or type a bus stop number, and the sign shows what's leaving next, counting down on your device's own clock. Everything is built from the free, official public transport timetable for Perth (GTFS). There is no server, no account and no tracking: the timetable is read on your device and saved there.

> Times are **scheduled**, not live. There is no public real-time feed, so delays and replacement-bus notices are not shown.

## The boards

### Train board (`train-board.html`)

Modelled on the platform displays at Perth train stations.

- **Any station**, picked from a list grouped by line (Armadale, Thornlie-Cockburn, Mandurah, Yanchep and the rest), plus an A–Z list.
- **Two signs side by side**, one per platform, with every line through the station merged in time order. At Beckenham, Platform 2 shows Byford and Cockburn Central trains interleaved, the same as the real board.
- **Next three services**: the next train large, the following two under "Then:".
- **Scrolling "Stops at" line** listing every station to the end of the line.
- **Arriving behaviour**: minutes round up like the real boards (never 0). At 1 minute the number and destination flash orange every 650 ms, and the train stays on the sign until 25 seconds after its scheduled time.
- **Demo mode**: 5 minutes of trains between Perth and Cannington at Victoria Park, no timetable needed. In demo the whole sign flashes orange.
- Full screen (landscape, screen kept awake), hide settings, text that sizes itself to fill the screen.

### Bus stop board (`bus-board.html`)

Modelled on the e-paper signs at city bus stops.

- **Type the stop number** shown at the bus stop (for example `12808`).
- **Next six departures** from every route at that stop: route number, destination and minutes ("Now", "5 mins", or a clock time beyond 30 minutes).
- Full screen, hide settings, auto-sizing text.

## Getting started

1. Open the site (see *Hosting on GitHub Pages* below, or open `index.html` directly).
2. The boards look for the timetable automatically, in this order:
   1. `google_transit.zip` in the same folder as the HTML files
   2. `gtfs/google_transit.zip`
   3. a copy you imported earlier on this device
3. If none is found, tap **Download timetable** (it fetches `google_transit.zip`, about 26 MB), then tap **Import** and choose it. It's saved on your device.
4. Train board: tap **Pick station**. Bus board: type a stop number and tap **Show stop**.

The timetable is refreshed regularly. Replace the zip in the repository (or Import a new download) every few weeks; the boards notice a changed file and reload it.

Automatic loading only works when the page is served from a web address such as GitHub Pages. Opened straight from a phone's Downloads folder, use Import.

## Settings

Each board has a few constants near the top of its `<script>`:

| Board | Constant | Default | What it does |
|---|---|---|---|
| Train | `STAY_SECONDS` | `25` | Seconds a train stays (flashing 1) after its scheduled time |
| Train | `FLASH_MS` | `650` | Length of each flash step |
| Train | `SCROLL_MS` | `800` | Time to scroll one character of the stop list (higher = slower) |
| Train | `TIMER_OFFSET_SECONDS` | `0` | Shifts every countdown; negative gives a leave-early buffer |
| Train | `DEMO_MINUTES` | `5` | Length of demo mode |
| Bus | `MINUTES_UP_TO` | `30` | Show minutes up to this, then clock times |
| Bus | `STAY_SECONDS` | `30` | Seconds a bus stays on "Now" after its scheduled time |
| Bus | `ROWS` | `6` | Departures shown |

## Hosting on GitHub Pages

This repository is a plain static site, so GitHub Pages can host it as-is. To have the boards load the timetable automatically, upload `google_transit.zip` to the root of the repository (or into a `gtfs` folder). Then set **Settings › Pages › Build and deployment › Source** to *Deploy from a branch*, branch `main`, folder `/ (root)`. The site appears at `https://<username>.github.io/<repository>/`.

## Data and licence

**Timetable data is available free of charge from [www.transperth.wa.gov.au](https://www.transperth.wa.gov.au).**

- Download: <https://www.transperth.wa.gov.au/TimetablePDFs/GoogleTransit/Production/google_transit.zip>
- Licence agreement: <https://www.transperth.wa.gov.au/About/Spatial-Data-Access>

The licence allows the data to be used, reproduced and redistributed, so `google_transit.zip` may be included in this repository. Conditions include stating that the data is available free of charge from the address above, and not using the provider's trademarks or copyrighted materials in association with the data. See [`DATA-LICENCE.txt`](DATA-LICENCE.txt) for a summary; the published terms apply.

This project is independent and not endorsed by the data provider. It does not use live data from the provider's website.

Code: MIT licence, see [`LICENSE`](LICENSE).

## How it works

The browser opens the zip with [JSZip](https://stuk.github.io/jszip/), streams `stop_times.txt` (about 75 MB uncompressed) line by line, and keeps only what it needs: train trips for the train board, one stop's departures for the bus board. Service days come from `calendar.txt` and `calendar_dates.txt`, so public holidays and closures in the published timetable are respected. Saved data lives in the browser's localStorage and IndexedDB.

Fonts: Pixelify Sans and Source Sans 3 from Google Fonts.
