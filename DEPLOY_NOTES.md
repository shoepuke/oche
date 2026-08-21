# OCHE — updated build

## What's new (this update)

- **"Analyze your own data" screen redesigned around the actual current
  workflow.** The primary action is now a big "Download the tool" button
  linking straight to `oche_sync.zip` -- since that tool now does
  everything (pulls, saves locally, builds a personal offline stats
  page), uploading/pasting CSVs manually is no longer the main flow.
  That capability still exists, just tucked behind a collapsed "Already
  have data.csv (and legs.csv)? Upload or paste them instead ›" link --
  still useful for you directly (since you also use
  `n01_puller.py`/`export_to_oche.py`), and for anyone who already has
  properly-formatted files from elsewhere.
- Page copy refreshed to match (title, lead paragraph, hint text).

## What's new (previous update)

- **`oche_sync.py` now generates a fully personalized, standalone
  `index.html`** right in the person's `Oche` folder -- their data is
  baked directly into the file (using OCHE's existing offline-fallback
  mechanism), so it's genuinely double-click-and-see-your-stats, no
  server, no upload, no internet needed after the pull itself finishes.
  Uploading to oche.pages.dev is now optional, not required. The zip
  needs a third file for this to work: `oche_template.html` (a copy of
  `index.html` used as the injection template).

## What's new (previous update)

- **New: a single combined tool for friends to get their own data**,
  `oche_sync.py` (packaged as `oche_sync.zip` with its own README). One
  script instead of two -- it pulls from n01, computes all the stats,
  and writes both OCHE-ready files automatically. Only prompts for gid +
  display name, and only on the very first run; every run after that is
  silent and just updates what's new. Data lands in a fixed, predictable
  folder (`Documents\Oche\` on Windows, `~/Documents/Oche/` on Mac/Linux)
  with the actual upload-ready files in an `oche_data\` subfolder, so
  there's no ambiguity about which files to grab.
- **The "Analyze your own data" screen now links directly to this tool**
  (a download-the-zip / read-the-README prompt right at the top of the
  uploader), so anyone landing there for the first time isn't staring at
  a blank paste box with no idea how to get their data.
- **Deploy now needs two more static files** alongside the usual three:
  `oche_sync.zip` and `oche_sync_readme.md`, both just sitting next to
  `index.html` like any other static asset -- no server-side anything.

## Recommendation on running two OCHE deployments

Decided against it (for now) -- the "Analyze your own data" flow already
gives everyone the exact same full experience from the one deployment,
and maintaining two live sites would mean double deployment work on
every future change (which happens a lot). The "expanded-data" URL seen
during testing was just a Cloudflare preview-branch artifact, not meant
to be a permanent second site.

## What's new (previous update)

- **"Avg darts/leg" box repositioned** to the top-right of its panel,
  above the date-range selector, instead of sitting on its own line.
- **Dart-count buckets are now correctly turn-aligned.** Buckets are a
  fixed grid starting at dart 1 -- 1-3, 4-6, 7-9, 10-12, 13-15, and so on
  -- rather than shifting based on whatever the lowest value in the
  current data happened to be. This means "30-32" always genuinely means
  darts 30-32 (the 10th turn), consistent across every view instead of
  drifting depending on your data range. Leading/trailing all-empty
  buckets are trimmed so the chart doesn't open with a long dead stretch,
  but gaps in the middle of your real data still show as zero-height bars
  so the shape of the distribution reads correctly.

## What's new (previous update)

- **"Avg darts/leg" box on the leg-length chart.** Sits right above the
  1M/3M/6M/1Y/All selector for "Legs won, by dart count" and updates with
  whatever period you've got selected -- same underlying data as the
  histogram, just as a quick headline number.
- **Career trend chart now has a third line: "This period avg
  (running)".** A windowed running average that resets at the start of
  whatever's currently zoomed in (1M/3M/6M/1Y/All) and builds up
  match-by-match from there, alongside the existing "Career avg
  (running)" line, which stays a true all-time figure regardless of
  zoom. Lets you see whether recent form is trending up or down within
  the period, not just a single blended number for it -- e.g. zoom to 3M
  and watch whether that line is climbing or falling as the period
  progresses. The two lines naturally converge when zoomed out to "All",
  since the period and the whole career are the same thing at that point.

## What's new (previous update)

- **"Analyze your own data" now supports leg-by-leg data too.** Select or
  drag-drop both `data.csv` and `legs.csv` together (auto-detected by
  header row, order doesn't matter) to get the full experience -- Career
  tab, match-detail modal -- instead of the stripped-down view it gave
  before. There's also a second, optional paste box for people who'd
  rather copy/paste than upload a file. Uploading just one file still
  works exactly like before (backward compatible).
- **"Legs won, by dart count" chart is now zoomable** with the same
  1M/3M/6M/1Y/All range controls as the trend charts, so you can see
  whether your leg-finishing efficiency has been trending recently
  rather than only seeing an all-time blend. The other Career page stats
  (high checkout, career averages, checkout %) stay all-time.

## What's new (previous update)

- **Match-detail modal now leads with a nakka-style two-column stats
  table** (You vs Opponent: Legs, 3 Darts, First 9, 60+/80+/100+/120+/
  140+/170+/180's, High Finish, 100+ Finishes, Best Leg, Worst Leg,
  Checkout%) instead of jumping straight to leg-by-leg. The leg-by-leg
  breakdown is still there, just behind a "Show leg-by-leg breakdown ›"
  toggle underneath the stats.
- **These stats are now computed for your opponent too**, not just you.
  `data.csv` gained matching `Opp_*` columns alongside every existing
  `Your_*` one -- same underlying (validated) logic, just applied
  symmetrically now instead of scoped to you only.

## What's new (previous update)

- **Career tab** (third button next to Opponents/Events): high checkout,
  fewest darts for a won 501 leg, career 3-dart avg, career first-9 avg,
  and a true dart-weighted career checkout % (not an average-of-percentages).
  Two bar charts: every 100+ checkout by exact value, and legs won grouped
  by dart count (green bars under 30 darts), with a callout showing how
  many of your legs finished under 30 darts.
- **Zoomable trend charts**: both the career trend (Overview) and the
  per-opponent/event trend now have 1M / 3M / 6M / 1Y / All range buttons.
  Pick a window smaller than "All" and ‹ › buttons appear to scroll by
  that same window (e.g. month-by-month in 1M view). The running career
  average line stays a true cumulative average even when zoomed in.
- **Click a chart point → match detail.** Instead of just jumping to the
  opponent view, clicking a dot now opens a modal with the full leg-by-leg
  breakdown for that match (who won each leg, darts used, checkout value).
  A link inside the modal still lets you jump to the full head-to-head.

## Deploy: now FIVE files

    index.html            <- this file (deploy once, then never)
    data.csv               <- your match-level history (from export_to_oche.py)
    legs.csv                <- your leg-level detail (from export_to_oche.py)
    oche_sync.zip            <- NEW: the friend-facing tool (script + its own README, zipped)
    oche_sync_readme.md       <- NEW: same README, standalone, for the "read first" link

Both CSVs need to sit next to `index.html`, same as before. `legs.csv` is
what powers the Career tab's bar charts and the match-detail modal --
without it, those features degrade gracefully (empty-state messages
instead of errors), but you'll want it deployed for the new stuff to work.

`oche_sync.zip`/`oche_sync_readme.md` are static files, just drop them in
next to everything else -- no build step, nothing server-side.

Generate your own data.csv/legs.csv with the updated exporter:

    python3 export_to_oche.py --matches out/matches.csv --legs out/legs.csv \
        --my-name "Craig Westwood" --out-dir ./deploy

Then copy `deploy/data.csv` and `deploy/legs.csv` over the ones next to
`index.html` and redeploy, same loop as before.

## Notes

- `legs.csv` only has data for matches pulled with a full n01_puller.py
  run (not `--summary-only`), same as the other detailed stats.
- If you upload/paste your own CSV via the "Analyze your own data"
  button (someone else's data, not the owner's), the Career tab and
  match-detail modal correctly won't show the owner's leg data mixed in
  -- they'll just show empty-state messages since there's no matching
  legs file for a manually pasted dataset.
- This was tested with a Node-based harness that runs the actual parsing/
  calculation code (not a reimplementation) against synthetic CSVs and
  checks the results by hand -- covers CSV parsing, career stat math, bar
  chart bucketing, the match-detail modal, running-average computation,
  and date-range filtering. It hasn't been tested in a real browser, so
  it's worth a visual once-over after deploying, especially on mobile
  (the modal and range-control buttons haven't been checked at narrow
  widths).
