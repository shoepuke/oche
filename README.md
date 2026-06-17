# OCHE — darts head-to-head analyzer

A self-contained darts performance analyzer. It boots into **your** match history
(read from `data.csv`) and lets anyone else explore **their own** data in the
browser via an "Analyze your own data" button. No server, no database, no
accounts — everything runs client-side, and no data ever leaves the viewer's
machine.

## The two files

| File | What it is | How often you touch it |
|------|------------|------------------------|
| `index.html` | The whole app. | Deploy once, then never. |
| `data.csv` | Your match history. | Overwrite after each match. |

Keep both at the **same level** on your host (e.g. both at the site root), so
`index.html` can find `data.csv` sitting next to it.

## Deploy (one time)

Any static host works. The simplest options give you a URL, free HTTPS, and a
custom domain in a few minutes:

- **Cloudflare Pages** or **Netlify** — drag the folder onto their dashboard.
- **GitHub Pages** — push the two files to a repo, enable Pages in settings.
- **AWS** — `index.html` + `data.csv` in an S3 bucket, CloudFront in front for
  HTTPS and a custom domain. More moving parts; best if you want it inside an
  existing AWS footprint.

Then send people the link. They land on your stats; the button lets them try it
with their own data (nothing they load is saved anywhere).

## Update after each match

1. Open your Google Sheet and select the cells **including the header row**.
2. Copy, then paste into `data.csv`, replacing its entire contents. Save.
3. Upload `data.csv` to your host.

That's the whole loop. `index.html` is never edited.

The page fetches `data.csv` with caching disabled, so viewers see the new data
immediately. After you overwrite the file, a normal redeploy on
Pages/Netlify/GitHub Pages refreshes the host's edge cache automatically.

## Preview locally

Because the page fetches `data.csv`, double-clicking `index.html` won't work
(a `file://` page is blocked from reading local files). Instead, from the
folder containing both files:

```
python3 -m http.server
```

then open <http://localhost:8000/>.

## Want a data-less public version?

Deploy `index.html` **without** a `data.csv`. With no history file to load, the
page falls through to the uploader — a clean "bring your own data" tool with no
baked-in stats.

## CSV format

**Required columns** (the tool won't load without these):

- `Date` — any sortable date, e.g. `2025-09-20`
- `Opponent`
- `Legs_You`
- `Legs_Opp`

**Optional, but they unlock more:**

- `Event` — powers the Events view (drill into a league/tournament; events are
  ordered chronologically).
- `Your_3Dart_Avg`, `Your_First9`, `Your_Checkout%` — appear in the
  "Metric to track" dropdown and drive the trend chart and averages.

Column names are matched loosely (case and punctuation don't matter), and any
extra columns you keep in the sheet are simply ignored. Percent signs in the
checkout column are fine.

## Configuration (rarely needed)

Near the top of `index.html`, in the `<script>` block:

- `OWNER_NAME` — the name shown on the page (e.g. `"Craig"`).
- `DATA_URL` — path to the history file. Default `"./data.csv"`.
- `EMBEDDED_CSV` — optional fallback used only if `data.csv` can't be fetched
  (e.g. opened via `file://`). Normally left empty.

## Troubleshooting

- **Page shows the uploader instead of my stats.** `data.csv` wasn't found, or
  it isn't next to `index.html`. Check the path and that it deployed.
- **"Missing column(s)" error.** One of the four required columns isn't in the
  header row. Check the exact names.
- **A column I expected isn't charted.** Only `Your_3Dart_Avg`, `Your_First9`,
  and `Your_Checkout%` are picked up as metrics; everything else is carried but
  not plotted.
- **Old data still showing after an update.** Re-deploy (or purge the cache) on
  your host so its edge picks up the new `data.csv`.
