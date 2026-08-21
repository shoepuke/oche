# Get your darts stats into OCHE

This tool pulls your match history from n01darts.com (the "nakka" online
darts platform) and produces two files that OCHE (oche.pages.dev) uses
to show your stats -- your head-to-heads, your averages over time, high
checkouts, all of it.

## What you need

Just Python 3. If you don't have it, download it from
https://www.python.org/downloads/ (any recent version works). On
Windows, make sure you check the box that says "Add Python to PATH"
during install.

## How to run it

1. Unzip this folder somewhere convenient (your Desktop is fine).
2. Double-click `oche_sync.py`, or open a terminal/command prompt in
   this folder and run:

   ```
   python3 oche_sync.py
   ```

3. **The first time**, it'll ask you two questions:
   - Your exact display name as it appears on n01
   - Your n01darts "gid" (see below for how to find it)

   After that, it remembers both -- you'll never be asked again.

4. It'll then pull your whole match history. This can take a few
   minutes the first time (there's a small delay between requests to be
   polite to n01's server). Every time after this first run, it only
   pulls whatever's new since last time, which takes just a few seconds.

5. When it's done, **it'll tell you where your personal stats page is**
   — just double-click it (it's a real `index.html`, sitting right in
   your Oche folder). Your data is baked directly into the file, so it
   works completely offline, no server, no upload step.

   It'll also mention a second set of files if you'd like to add your
   data to oche.pages.dev instead/as well -- click "Analyze your own
   data" there and select both.

That's it. Run it again anytime you want your stats updated -- just
double-click `oche_sync.py` again, no questions asked the second time
onward, and your local `index.html` gets refreshed automatically too.

## Finding your gid

1. Go to https://n01darts.com (there's no separate login button -- you
   authenticate as part of step 2).
2. Create an online game. You'll be prompted to sign in via Google,
   Facebook, or X. Check the box for **"require a secret to join"** so
   nobody else joins and starts a match on you -- you don't need to
   actually play anything.
3. In the list of available online matches, click **your own name**.
4. This opens your stats -- click **History**.
5. Look at the URL in your browser's address bar. Copy the number after
   `gid=`. That's it -- paste that in when the script asks.

Your gid identifies your n01/Google account, so treat it as personal
(don't post it publicly), but it can only be used to *read* match
history, not log in anywhere, so it's low-risk to use.

## Where your data goes

Everything lives in a folder called "Oche" in your Documents folder:

- **Windows:** `C:\Users\<you>\Documents\Oche\`
- **Mac:** `/Users/<you>/Documents/Oche/`
- **Linux:** `~/Documents/Oche/` (or `~/Oche/` if you don't have a
  Documents folder)

Inside, you'll find:

- **`index.html` -- your personal stats page. Just double-click this.**
  Your data is baked right into the file, so it works completely
  offline.
- `config.json` -- your saved name/gid, so you're never asked again
- `excluded_match_ids.txt` -- **optional.** Want a specific match left out
  of your stats entirely (a weird one-off game, whatever)? Create this
  file (or add to it) with the match's `Mid` on its own line -- find the
  Mid in `oche_data\data.csv`. Add `# a note to yourself` after it if
  you want to remember why later. The match stays in your raw pulled
  data either way, it's just left out of what OCHE sees.
- `raw_pull\` -- the script's own working data; you never need to touch
  this, but it's what makes updates fast (it remembers every match
  it's already fetched, so re-runs are quick and don't hammer n01's
  server)
- `oche_data\` -- `data.csv` and `legs.csv`, only needed if you'd
  rather upload your data to oche.pages.dev than use your own local
  `index.html`

## A note on game types

By default, only standard 501 matches count toward your stats -- other
game types (like a 301 tournament) get automatically left out, since
they're not really comparable (much shorter legs, different checkout
odds, etc.) and would just muddy a "progress over time" view. Your raw
pulled data always has everything regardless. If you ever want every
game type included, run with `--start-score all`.

## A couple of honest notes

- This talks to n01's internal backend, not an official public API, so
  it's worth keeping this low-key rather than posting it somewhere very
  public -- share it directly with people who'll actually use it.
- If you ever change your display name on n01, run the script with
  `--reconfigure` once to update your saved name (or just delete
  `config.json` and it'll ask again next run).
- Nothing about this uploads your data anywhere except wherever *you*
  choose to paste/upload it (like OCHE). It only ever talks to n01's
  server to fetch your own match history.
