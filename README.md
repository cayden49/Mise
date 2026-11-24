# Mise: a recipe lab notebook you own

A free, offline recipe app for Windows, Mac and iPhone. There's no account, no subscription, no App Store and no server. Every recipe is a plain folder of JSON and JPEG files in a place you pick.

```
Mise/                                  ← put this in iCloud Drive
  pantry-densities.json                ← your grams-per-cup numbers
  recipes/
    neapolitan-pizza-dough--r8x2k.../
      recipe.json                      ← the recipe
      cook-log.json                    ← append-only cook log
      versions.json                    ← v1.0, v1.1, … snapshots
      photos/ph….jpg  ph….thumb.jpg    ← ~150–250 KB + ~25 KB thumbnail
  _bundles/                            ← iPhone sync drop-box
```

---

## Setup

### Windows PC or Mac: about 2 minutes
1. Put the `Mise` app folder anywhere, for example `Documents/Mise App`.
2. Open `index.html` in **Chrome** or **Edge**. (Safari and Firefox work too, but they can't save to a folder; see "On-device mode".)
3. Click **Choose library folder…** and pick or create a folder named `Mise` inside **iCloud Drive**:
   - **Windows:** install *iCloud for Windows* from the Microsoft Store, then use `C:\Users\<you>\iCloudDrive\Mise`
   - **Mac:** `iCloud Drive › Mise`. Then right-click that folder and choose **Keep Downloaded**, so macOS doesn't offload recipes to the cloud.
4. Optional: if you use the hosted address from the iPhone steps below, Chrome and Edge can **install** it as an app with its own window and Dock/taskbar icon (⋮ menu › Install).

The browser asks once per session before reopening the folder. If it offers **Allow on every visit**, pick that.

Dropbox, OneDrive, Google Drive or Syncthing work just as well as iCloud. Mise only sees a folder.

### iPhone: about 10 minutes, one time
iOS only runs web apps from a web address, so put the app files on free static hosting. **Only the app's code goes there. Your recipes never leave your devices.**

1. Make a free account at github.com and create a new **public** repository called `mise`.
2. Choose **Add file › Upload files** and drag in everything from this folder (`index.html`, `sw.js`, `manifest.webmanifest`, the icons). Commit.
3. Go to **Settings › Pages**. Set *Source* to **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After about a minute, open `https://<your-username>.github.io/mise/` in **Safari** on your iPhone.
5. Tap **Share › Add to Home Screen**. This matters: iOS keeps a Home Screen app's storage permanently and runs it offline.

The same URL works on your computers too, if you'd rather not keep the file locally.

---

## Syncing

| Device | How it syncs |
|---|---|
| Windows / Mac (Chrome, Edge) | Reads and writes the library folder directly. iCloud Drive keeps the computers in step. Automatic. |
| iPhone | Apple doesn't let web apps open iCloud folders by themselves, so the phone keeps its own copy and swaps **bundle files** through `iCloud Drive › Mise › _bundles`. |

**Phone to computer:** Settings › **Export changes** › Share sheet › **Save to Files** › `iCloud Drive/Mise/_bundles`. Your computers merge it automatically the next time Mise opens (or click **Check _bundles now**).

**Computer to phone:** On the computer, Settings › **Export changes** (it writes straight into `_bundles`). On the phone, Settings › **Import bundle…** › pick that file.

Merging is safe to repeat and never overwrites:
- **Recipes:** the newest edit wins, and photos from both sides are kept.
- **Cook logs and versions:** append-only and combined by ID, so notes you took offline in the kitchen are never lost.
- **iCloud conflict copies** (`recipe 2.json`, `cook-log 2.json`, and Dropbox's "conflicted copy") are detected, merged and cleaned up when the library opens. If two devices edited the *same* recipe, the losing edit is kept as a version snapshot marked `·conflict`, so you can compare or restore it.

### On-device mode
Safari, Firefox and iPhone store recipes inside the browser and sync with bundles, as described above. Settings › **Download full backup** gives you one file with everything; keep it somewhere other than the sync folder.

---

## Features

**Baking mode (baker's math).** Mark base flour(s) and they count as 100%. Mise shows hydration, salt, yeast, fat, sugar and prefermented-flour percentages. Levain is split into its flour and water (set levain hydration in the editor). Total hydration counts milk at about 87% water and eggs at about 75%.
**Solver:** *Make 3 × 280 g of dough* (or *of flour*), with an optional % loss for what sticks to the bowl.

**Cooking mode (smart scaling).** ½× to 3× buttons, a servings stepper, or type any number.
**Fixed items** (deep-frying oil, pasta water, dusting flour) never scale. The paste importer flags them automatically.

**Kitchen rounding.** Whole grams from 10 g up, 0.5 g between 2 and 10 g, and 0.1 g under 2 g (for yeast on a pocket scale). Spoons and cups snap to real measures: `0.33 tbsp → 1 tsp`, `5 tbsp → ⅓ cup`, `¾ cup + 2 tbsp`. Use **As written / Metric / US cups** to convert, based on your own `pantry-densities.json`. Weigh a level cup of your flour and enter it in Pantry.

**Sub-recipes.** In the editor, click **↳ link** on an ingredient to use another recipe. The amount can be in batches (`1 batch`), by weight (`280 g`), or in the other recipe's yield unit (`1 ball` of a 3-dough-ball recipe = ⅓ batch). Sub-recipes scale with the parent, and their steps come first in cooking mode.

**Cooking mode.** The screen stays awake, the text is large and high-contrast (A−/A+), you tap an ingredient to strike it off, tap a step to jump to it, and use ◀ ▶ (or arrow keys / space). Your place and checklist survive a reload.

**Timers.** `{45 min}`, `{1 h 30 min}`, `{2-3 hours}`, or plain "bake 25 minutes" in a step all become tap-to-start timers. Several can run at once in the tray, and they keep accurate time through reloads. Each has pause, +1 min and restart, plus an alarm sound, vibration where supported, and a desktop notification.

**Paste importer.** Paste a whole web page, note or video description. Mise ignores noise like "Jump to recipe", ratings and nutrition. It finds the title, yield, times, sections ("For the filling:"), quantities with fractions, ranges and unicode ½, units, notes and equipment. When a line gives both `2¼ cups (280 g)`, it keeps the grams.

**Search and filters.** Full-text search over titles, ingredients, steps and notes (put `-` before a word to exclude it, and press `/` to focus the search box). Filter by Cooking/Baking, Equipment, Tags, Rating, and Last cooked (this week, this month, never, or not in 3+ months). Sort by recently cooked, most cooked or top rated.

**Lab notebook.**
- **Versions:** save v1.1, v1.2 or v2.0 with a change note. A badge shows when the recipe has changed since its last version. **What changed** diffs two versions. **Restore** first auto-saves your current state, so restoring is never destructive.
- **Log a cook:** date, ★ rating, room/dough temperature, humidity, oven quirks, substitutions, notes, the scale you used, and which version you cooked, plus photos.

**Photos.** Resized on import to a 1600 px long edge at about 150–250 KB, with a 480 px thumbnail for the library grid. Kinds are dish, crumb, step, process and plating; you choose the cover.

**Also:** Duplicate, share a single recipe as a file (send it to a friend running Mise), Print, Trash with restore, light "paper" and dark "night kitchen" themes, and a text-size setting.

## Honest limits
- iPhone sync takes a couple of taps each way. That's an Apple rule for web apps; only a paid native app can watch an iCloud folder.
- On iPhone, timer alarms only sound while Mise is on screen. Keep it open; the screen-awake setting helps.
- Keeping the screen awake needs a recent browser: any current Chrome or Edge, or Safari on iOS 16.4+. Older iOS versions had a bug that stopped it working in Home Screen apps; update iOS if the ☀ icon won't turn on.
- HEIC photos import fine on iPhone and Mac. On Windows, Chrome can't read HEIC, so export as JPEG.

## Files in this folder
`index.html` is the whole app (about 200 KB, no internet needed). `sw.js` and `manifest.webmanifest` make it installable and offline when hosted. The rest are icons. The `source/` folder has the readable code and tests. To customize, edit `source/src/` and run `python3 source/build.py`.
