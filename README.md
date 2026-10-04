<p align="center">
  <img src="docs/logo.png" alt="Butter Quest Tracker Fan Update logo" width="160">
</p>

<h1 align="center">Butter Quest Tracker Fan Update</h1>

<p align="center">
  A clean, customizable quest tracker for World of Warcraft, updated for <strong>WoW Forever</strong>.
</p>

> **Unofficial fan update.** This is a community-maintained update of
> [Butter Quest Tracker](https://github.com/butter-cookie-kitkat/ButterQuestTracker) by Butter Cookie Kitkat,
> used under its MIT license. It is not made, endorsed or supported by the original author, so please report
> problems with this version here and not to them.

Butter Quest Tracker (BQT) replaces Blizzard's quest tracker with a tidy list that you control: what it shows,
how it is grouped, how it looks and where it sits on screen.

<!-- Add screenshots here, for example:
![The tracker with zone headers and quest levels](docs/screenshot-tracker.png)
![The options panel](docs/screenshot-options.png)
-->

## What's new in the Fan Update

- **Works on WoW Forever.** Updated for the new client's modern quest API.
- **Zone grouping.** Quests sit under collapsible zone headers. Your current zone comes first, the rest follow A to Z. A setting switches back to ordering zones by quest order.
- **Show Quest Level.** Optionally show `[12] Quest Name` or `Quest Name (12)`.
- **Clearer difficulty colors.** Color each quest name by how hard it is for your level, like the quest log does.
- **No tracking limit.** BQT keeps its own list of tracked quests, so Blizzard's cap does not apply. It still mirrors the list into Blizzard's and reacts when you track or untrack from the quest log.
- **Safer startup.** If BQT ever fails to start, you get Blizzard's default tracker back instead of none.
- **Bug fixes.** Quest names containing `%`, quests without objectives repeatedly looking "updated", a broken locale fallback, moved popup fields and stale quest log indexes.

## Features

- Move and lock the tracker anywhere on screen
- Collapsible zone headers
- Filter quests to your current zone or subzone
- Manually track or untrack quests
- Sort by level, percent complete, recently updated, or quest proximity (needs a quest helper addon)
- Color quest names by difficulty and optionally show their level
- Adjust font size, colors, padding and the quest name format
- Alt-click a quest for its Wowhead link, ctrl-click to link it in chat, shift-click to untrack it
- Right-click a quest to view, share or abandon it
- Type `/bqt` for the settings, `/bqt status` for diagnostics

## Compatibility

| Client | Status |
| --- | --- |
| **WoW Forever** | Played and tested in game |
| Classic Era, Cataclysm / Mists Classic, Retail | Covered by automated tests against simulated clients, not yet played on live servers |

BQT checks which APIs actually exist instead of guessing from the version number, so it should keep working through
most patches. If something looks wrong on your client, please open an issue and include the `/bqt status` output.

Optional quest helper addons: [Questie](https://www.curseforge.com/wow/addons/questie) and
[ClassicCodex](https://www.curseforge.com/wow/addons/ClassicCodex) (used for sorting by quest proximity).
Works with the quest log replacements
[Classic Quest Log](https://www.curseforge.com/wow/addons/classic-quest-log),
[QuestLogEx](https://www.wowinterface.com/downloads/info24980-QuestLogEx.html) and
[QuestGuru](https://www.curseforge.com/wow/addons/questguru_classic).

## Installing

1. Delete any old `Interface/AddOns/ButterQuestTracker` folder. Your settings are kept, they live in `WTF`.
2. Download the latest release and copy the `ButterQuestTracker` folder into the `Interface/AddOns` folder of your client
   (`_classic_era_`, `_classic_`, `_retail_`, or the WoW Forever folder).
3. The folder must be named exactly `ButterQuestTracker`. GitHub's "Download ZIP" names it `ButterQuestTracker-master`, so rename it
   or use the zip from the Releases page.
4. Start the game. If the addon is marked "out of date" after a patch, tick **Load out of date AddOns** on the character screen.

All libraries (Ace3) are bundled, nothing else needs installing.

## Using it

| Action | What it does |
| --- | --- |
| `/bqt` | Open the settings |
| `/bqt status` | Print the detected client and quest APIs (include this in bug reports) |
| `/bqt reset` | Clear your manual track and untrack choices |
| Left-click the header | Collapse or expand the tracker |
| Right-click the header | Open the settings |
| Click a quest | Open it in the quest log |
| Shift-click a quest | Untrack it |
| Alt-click a quest | Show its Wowhead link |
| Ctrl-click a quest | Link it in chat |
| Right-click a quest | Quest menu (view, share, abandon) |

Most options are under **Visual Settings** (header, zone, quest and objective text, fonts, colors) and
**Filters & Sorting**. If you track or untrack a quest by hand and want filtering to apply to it again, use
**Filters & Sorting > Reset Tracking Overrides**.

## Reporting bugs

Open an issue and include:

- the output of `/bqt status`
- the full text of any Lua error (BugSack or BugGrabber capture these)
- which client you play on, and what you were doing when it happened

## For developers

Blizzard changes the addon API with most patches, so BQT is built to be cheap to fix:

1. **New patch marks the addon out of date:** add the new interface number to the first line of `ButterQuestTracker.toc`.
   Find it in game with `/dump (select(4, GetBuildInfo()))`.
2. **A quest, map or UI function is renamed or removed:** fix it in `Compat/Compat.lua`. Everything BQT does with the game's
   quest, map or UI APIs goes through that file. Each function tries the modern API first, falls back to older ones, and is
   guarded so one missing function cannot take the whole addon down.
3. **Check your change without logging in:**
   ```
   pip install lupa
   python3 tests/run_tests.py
   ```
   The tests load the whole addon against fake clients (Classic Era style, modern, and one with no usable quest log) and drive
   tracking, clicks, menus and options. See [`tests/README.md`](tests/README.md). They are not a WoW emulator, so a passing run
   does not replace trying the addon in the real client.

## Credits and license

- Original addon: **Butter Quest Tracker** by Butter Cookie Kitkat, (c) 2019, MIT license. The [`LICENSE`](LICENSE) file is unchanged.
- Fan Update changes are released under the same MIT license.
- Bundled libraries keep their own licenses: Ace3 (see [`Libs/Ace3-LICENSE.txt`](Libs/Ace3-LICENSE.txt)), LibStub and CallbackHandler.
