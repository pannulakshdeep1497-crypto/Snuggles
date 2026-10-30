# Snuggles: a calm baby log for two phones

Snuggles logs sleep, feeds, pumping, diapers, medicines, growth and
appointments. It installs to your home screen, works offline, and can sync
between two phones through a Google Sheet in your own Google Drive.

## What's in this folder
- `index.html`: the whole app
- `sw.js`, `manifest.webmanifest`, `icon-*.png`: offline use and home-screen install
- `Code.gs`: the Google Apps Script that turns a Google Sheet into the sync hub

---------------------------------------------------------------------------
## Step 1. Make the Google Sheet (one of you, about 5 minutes)

1. Go to https://sheets.google.com and create a blank sheet. Name it "Snuggles".
2. In the sheet: Extensions > Apps Script.
3. Delete what's in the editor, paste in all of `Code.gs`.
4. On the line `const SECRET = '...'`, replace the text with your own long
   phrase, e.g. `'blue-otter-sings-at-3am-4417'`. This is your **sync key**.
5. Click Save (disk icon).
6. Click **Deploy > New deployment**. Click the gear next to "Select type" and
   choose **Web app**. Set:
   - Execute as: **Me**
   - Who has access: **Anyone**
7. Click Deploy, then Authorize access. Google will warn that the app isn't
   verified (it's your own script): click Advanced > Go to project > Allow.
8. Copy the **Web app URL**. It ends in `/exec`.

"Anyone" lets the app reach the script without a Google sign-in on each
phone. The script refuses every request that doesn't carry your sync key,
so keep the URL and key private.

## Step 2. Put the app online (about 5 minutes)

The app has to run from its own web address for sync to work (the version
inside Claude can't talk to Google). Easiest free option:

1. Go to https://app.netlify.com/drop (free account).
2. Drag this whole `snuggles` folder onto the page.
3. You get an address like `https://something.netlify.app`. Optionally rename
   it under Site settings.

GitHub Pages or Cloudflare Pages work the same way.

## Step 3. Install on your phone

1. Open the address in **Safari** (iPhone) or **Chrome** (Android).
2. iPhone: Share > Add to Home Screen. Android: menu > Install app.
3. Open Snuggles from the home screen. Settings > Sync between phones.
4. Paste the Web app URL and your sync key, tap **Connect**.
   Anything already logged on this phone uploads to the sheet.
5. Tap **Copy for the other phone** and send that line to your partner
   privately (e.g. by text).

## Step 4. Install on your partner's phone

1. Same as Step 3, items 1 to 3.
2. Paste the line you were sent into the **URL** box (it fills in the key too),
   tap **Connect**. All entries download.

Done. Each phone shows "Synced just now" under the baby's name.

---------------------------------------------------------------------------
## How syncing behaves
- Sync runs when the app opens, about 2 seconds after any entry or edit, and
  every 30 seconds while the app is open. Phones can't sync while the app is
  closed, but opening it catches up straight away.
- Timers sync too: if one of you starts a feed or sleep timer, the other phone
  shows it running and can stop it.
- Offline? Keep logging. Changes wait on the phone and upload when you're back
  online.
- If both of you edit the same entry, the most recent edit wins.
- Deleting an entry deletes it on both phones (Undo brings it back on both).
- "Delete all data on this phone" in Settings only clears that phone and
  disconnects it; the sheet and the other phone keep everything.
- Baby name, birthday, units and night-light settings are per phone.

## The sheet
A tab called "Snuggles" appears after the first sync. "when" and "summary"
are there so you can read the log in Google Sheets; "data" is what the app
uses. Please don't edit, sort or delete rows by hand. To start over, make a
new sheet and script and reconnect both phones.

## Troubleshooting
- "The sync key doesn't match": the key on the phone must exactly equal SECRET.
- "Set your own SECRET": you deployed before changing SECRET. Change it, then
  Deploy > Manage deployments > pencil > Version: New version > Deploy.
- "Didn't answer correctly": use the URL ending in `/exec` and make sure
  "Who has access" is set to Anyone.
- After editing Code.gs, always deploy a **new version** under Manage
  deployments (a brand-new deployment gets a new URL).

## Updating the app
Edit `index.html`, change `VERSION` in `sw.js` (e.g. snuggles-v2), and drag
the folder to Netlify again. Phones pick it up the next time the app opens.
