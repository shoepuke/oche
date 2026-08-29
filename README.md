# OCHE

A darts head-to-head and form-tracking dashboard for [n01darts.com](https://n01darts.com) ("nakka") players. Single-page, no backend, no build step — just a CSV-driven HTML file you can host anywhere.

**Live:** [oche.pages.dev](https://oche.pages.dev)

## What it does

Point OCHE at your match history and it gives you:

- **Opponents view** — head-to-head record, form (W/L/D pips), trend chart, and a full meeting-by-meeting table for any opponent you've faced.
- **Events view** — same, but grouped by tournament/event instead of opponent.
- **Career view** — high checkout, fewest darts in a won 501 leg, career 3-dart average, career first-9 average, an estimated dart-weighted career checkout %, plus two charts: every 100+ checkout by exact value, and legs won grouped by dart count (zoomable to 1M/3M/6M/1Y/All, with a running "this period" average alongside your all-time average).
- **Match detail** — click any point on a trend chart to open a nakka-style stats table (You vs Opponent: 3-dart avg, First 9, scoring buckets, high finish, checkout %) with the full leg-by-leg breakdown underneath.
- **Bring-your-own-data** — upload or paste a CSV (or two — a leg-level file unlocks the Career tab and match detail) with zero setup, or use the built-in demo dataset to try it out first.

## Getting your own data in

If you play on n01darts.com, the easiest path is the companion tool, **`oche_sync.py`** (packaged as `oche_sync.zip`, with its own README inside):

1. Download and unzip it.
2. Run `python3 oche_sync.py`. First run asks for your n01 display name, which account you signed into n01 with (Google, Facebook, or X), and the id for that account (see the zip's README for how to find it) — every run after that is silent and just pulls what's new.
3. It builds you a fully personal, offline `index.html` (your data baked right in — no server needed), plus `data.csv`/`legs.csv` if you'd rather add them to a hosted copy of OCHE instead.

No n01darts account, or want to build your own data by hand? See the schema below — any spreadsheet with the right columns works.

## Data schema

**`data.csv`** (required columns in **bold**):

| Column | Notes |
|---|---|
| **`Date`** | Any sortable date format |
| **`Opponent`** | |
| **`Legs_You`** / **`Legs_Opp`** | |
| `Event` | Groups matches in the Events view |
| `Your_3Dart_Avg`, `Your_First9`, `Your_Checkout%` | Unlock the metric-tracking dropdown |
| `Your_High_Finish`, `Your_Checkout_Makes`, `Your_Checkout_Attempts` | Power the Career page -- see note below on accuracy |
| `Your_60_Plus` … `Your_180s` | Scoring-band counts (cumulative — a 140 counts toward 60+/80+/100+/120+/140+ all at once) |
| `Your_100_Plus_Finishes`, `Your_Best_Leg_Darts`, `Your_Worst_Leg_Darts` | |
| `Opp_*` | Same set of columns, opponent's side — powers the match-detail stats table |
| `Mid` | A stable match ID, used to join against `legs.csv` for match detail |

**`legs.csv`** (optional, unlocks the Career tab and leg-by-leg match detail):

| Column | Notes |
|---|---|
| `Mid`, `Leg`, `Player` (`You`/`Opponent`), `Won` | Required |
| `Darts`, `CheckoutValue`, `StartScore` | Needed for the Career page's checkout/leg-length charts |

Column names are matched loosely (case/spacing-insensitive), so a hand-edited Google Sheet export works fine as long as the header row is close.

## Deploying your own copy

Five files, all static, no build step:

```
index.html          <- this repo
data.csv             <- your match history
legs.csv              <- your leg-level detail (optional but recommended)
oche_sync.zip          <- the data-puller tool + its README, for others to grab
oche_sync_readme.md     <- same README, standalone (for the "read first" link)
```

Push them to Cloudflare Pages, Netlify, GitHub Pages, or any static host. To preview locally before deploying, `fetch()` won't work over `file://`, so run a tiny server in the folder instead:

```
python3 -m http.server
```

then visit `http://localhost:8000/`.

## Notes

- **Checkout attempts/percentage are a best-guess estimate, not a hard
  fact.** n01's match data only exposes each turn's total score, never
  the individual dart values that made it up, so there's no way to know
  for certain which turns were genuinely thrown at a double versus just
  ordinary scoring. `oche_sync.py`'s crediting logic (see its README) is
  tuned against real match history and generally lands close, but any
  single match can still be off, sometimes noticeably. Everything else
  in OCHE (legs, darts, checkout *values*, 3-dart average, scoring bands)
  comes straight from n01's own recorded data and is exact.
- Talks to n01's internal backend, not an official public API — this could break if n01 changes something. Keep your own copy of the puller tool up to date if that happens.
- Everything runs client-side. Your data never leaves your browser except however *you* choose to host/share the CSVs.
- See `DEPLOY_NOTES.md` for a running changelog of what's shipped and when.
