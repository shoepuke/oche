# Get your darts stats into OCHE

This tool pulls your match history from n01darts.com (the "nakka" online
darts platform) and produces two files that OCHE (oche.pages.dev) uses
to show your stats -- your head-to-heads, your averages over time, high
checkouts, all of it.

## What you need

Just Python 3.

- **Windows:** if you don't have it, download from
  https://www.python.org/downloads/ (any recent version works) — make
  sure you check the box that says "Add Python to PATH" during install.
- **Mac:** see "Running it on a Mac" below — installing Python and
  actually running the script both work a bit differently than Windows.

## How to run it

1. Unzip this folder somewhere convenient (your Desktop is fine). On
   Mac/Windows this means double-clicking the `.zip` file in Finder or
   File Explorer -- it creates a **new folder** sitting right next to
   it with the same name. Make sure you can see that new folder (not
   just the original `.zip`) before continuing -- you'll need it, not
   the zip file itself.
2. Run it:
   - **Windows:** double-click `oche_sync.py`.
   - **Mac:** double-clicking usually won't work — see "Running it on a
     Mac" below.
   - **Linux:** open a terminal in this folder and run
     `python3 oche_sync.py`.
3. **The first time**, it'll ask you a few quick questions:
   - Your exact display name as it appears on n01
   - Which account you use to sign into n01 -- Google, Facebook, or X/Twitter
   - Your id for that account (see below for how to find it)

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

## Running it on a Mac

Two things work differently than Windows: getting Python installed, and
actually running the script (double-clicking a `.py` file doesn't do
anything useful on a Mac by default -- you'll use Terminal instead,
which is much less scary than it sounds).

1. **Check if you already have Python 3.** Press `Cmd + Space`, type
   `Terminal`, hit Enter -- this opens the Terminal app. Type:

   ```
   python3 --version
   ```

   If that prints a version number like `Python 3.11.x`, you're set --
   skip to step 3. (Older Macs came with a "Python 2" pre-installed;
   that's a different, unrelated thing and won't work here, hence the
   check.)

2. **If you don't have it**, go to
   https://www.python.org/downloads/macos/, download the installer, and
   run it, clicking through with the defaults. If macOS shows a warning
   about an "unidentified developer," go to **System Settings → Privacy
   & Security** and click **"Open Anyway"** next to the blocked-app
   notice, then try opening the installer again.

   **One more one-time step, easy to miss:** after installing, go to
   Finder → Applications → the "Python 3.x" folder it created, and
   double-click **"Install Certificates.command"** inside it. A Terminal
   window will briefly appear and close on its own -- that's it working.
   Skipping this causes a `CERTIFICATE_VERIFY_FAILED` error the first
   time the script tries to reach n01's server (harmless and fixable by
   just doing this step and running the script again, but easiest to
   just do it now).

3. **Navigate to the unzipped folder in Terminal.** This is two
   separate actions, in this exact order:

   - First, click into the Terminal window and type `cd ` -- just the
     letters `c`, `d`, then one space. **Don't press Enter yet, and
     don't type anything else.**
   - Then, in Finder, drag the unzipped **folder** (not the `.zip`
     file -- look for a plain folder icon, not a zipper/archive icon)
     onto the Terminal window. This pastes the correct path in after
     `cd `. *Now* press Enter.

   If you type things in the wrong order, or drag the `.zip` file
   instead of the unzipped folder, you'll see something like
   `zsh: command not found` -- that's the signal something above went
   sideways; just start again from the `cd ` step.

4. **Run it:**

   ```
   python3 oche_sync.py
   ```

Every time after this first run, just repeat steps 3-4 (or keep the
Terminal window open and press the up arrow to bring back the last
command instead of retyping it).

## Finding your id

1. Go to https://n01darts.com (there's no separate login button -- you
   authenticate as part of step 2).
2. Create an online game. You'll be prompted to sign in via Google,
   Facebook, or X. Check the box for **"require a secret to join"** so
   nobody else joins and starts a match on you -- you don't need to
   actually play anything.
3. In the list of available online matches, click **your own name**.
4. This opens your stats -- click **History**.
5. Look at the URL in your browser's address bar. Copy the number after
   `gid=`, `fid=`, or `tid=` -- whichever one shows up. That's it --
   paste that in when the script asks (and tell it which one it was, so
   it knows to look for `gid=`, `fid=`, or `tid=` next time).

Which one you see depends on how you signed in: Google gives `gid`,
Facebook gives `fid`, X/Twitter gives `tid`. They're different account
systems, so it does matter which one you use.

Your id identifies your n01/Google/Facebook/X account, so treat it as
personal (don't post it publicly), but it can only be used to *read* match
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
- `config.json` -- your saved name/id, so you're never asked again
- `excluded_match_ids.txt` -- **optional.** Already sitting here with
  format instructions and an example inside it. Want a specific match
  left out of your stats entirely (a weird one-off game, whatever)? Open
  it and add the match's `Mid` on its own line -- find the Mid in
  `oche_data\data.csv`. The match stays in your raw pulled data either
  way, it's just left out of what OCHE sees.
- `raw_pull\` -- the script's own working data; you never need to touch
  this, but it's what makes updates fast (it remembers every match
  it's already fetched, so re-runs are quick and don't hammer n01's
  server)
- `oche_data\` -- `data.csv` and `legs.csv`, only needed if you'd
  rather upload your data to oche.pages.dev than use your own local
  `index.html`

## A note on game types

By default, only standard 501 matches count toward your stats -- other
game types (like a 301 tournament, or Cricket) get automatically left
out, since they're not really comparable (much shorter legs, different
checkout odds, or a totally different scoring system entirely) and would
just muddy a "progress over time" view. Your raw pulled data always has
everything regardless. If you ever want every game type included, run
with `--start-score all`.

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
