# Umamusume Race Tracker

A single-file, client-side web app to plan, track, and review races for any Umamusume. No backend. Data stays in the browser and can be exported to JSON.

> File: `umamusume-race-tracker.html` (HTML, CSS, and JS in one file)

---

## Contents

* [Features](#features)
* [Quick Start](#quick-start)
* [How to Use](#how-to-use)

  * [Select or Add an Umamusume](#select-or-add-an-umamusume)
  * [Edit Umamusume Details](#edit-umamusume-details)
  * [Views](#views)
  * [Filters and Search](#filters-and-search)
  * [Mark Results](#mark-results)
  * [Career Tools and Crown Titles](#career-tools-and-crown-titles)
  * [Calendar and Popover](#calendar-and-popover)
  * [Import, Export, and Reset](#import-export-and-reset)
* [Data Model](#data-model)

  * [Local Storage Keys](#local-storage-keys)
  * [Race Key Format](#race-key-format)
  * [Races and Crowns](#races-and-crowns)
* [Customization](#customization)
* [Styling](#styling)
* [Privacy and Security](#privacy-and-security)
* [Browser Support](#browser-support)
* [Known Limitations](#known-limitations)
* [Changelog](#changelog)
* [License](#license)

---

## Features

* **Zero-setup**
  Open the HTML file in a modern browser. No build step. No external libraries.

* **Umamusume management**

  * Preloaded list of well-known Umamusume.
  * Add new Umamusume from the selector.
  * Per-Umamusume data is stored separately.

* **Details editor**

  * Photo upload with instant save. Click the photo to view a large version.
  * Track Aptitude: Turf and Dirt ranks A to G.
  * Distance Aptitude: Sprint, Mile, Medium, Long ranks A to G.
  * **Pace Aptitude**: Front runner, Pace chaser, Late surger, End closer ranks A to G. Two strong paces are supported by setting letters.
  * Recommended Support Cards: exactly 6 slots with type tags `SPD`, `STAM`, `PWR`, `GUTS`, `WIT`.
  * Alternative Support Cards: unlimited rows, add or remove as needed.
  * Recommended Skills: unlimited rows, add or remove as needed.
  * Deck Notes: free text.

* **Dashboard**

  * Views: **Recommended**, **Career**, **All races**, **Calendar**.
  * Filters: Grade (G1, G2, G3), Track (Turf, Dirt), Distance (Sprint, Mile, Medium, Long), Year (Junior, Classic, Senior, EX).
  * Quick Search across race name, month, year, grade, distance, and track.
  * Sort on **Race** and **Year** headers. Sticky header for large lists.
  * Stats cards: total races in view, wins, and losses.
  * Row highlights:

    * Win and Loss coloring.
    * Focus for Crown title targets.

* **Career tools**

  * Add or remove any race from the Umamusume’s Career list.
  * Crown Titles section toggles race focus sets and highlights them in the table.

* **Calendar**

  * Month buckets, sorted in calendar order.
  * Click a race title to open a popover with one-click Career add or remove.

* **Persistence**

  * All data is saved per Umamusume in the browser `localStorage`.
  * Export and Import progress as JSON.
  * Reset marks or clear data for the selected Umamusume only.

* **UI quality**

  * Compact layout with responsive grids.
  * Skill editor layout prevents the add button from overlapping inputs.
  * Accessible dialogs for photo viewer, details editor, and career list.

---

## Quick Start

1. Download or copy `umamusume-race-tracker.html`.
2. Open it in a modern desktop browser.
3. Pick an Umamusume from the selector.
4. Click **Edit Umamusume Details** to set aptitudes, supports, skills, and notes.
5. Switch the **View** and apply **Filters** as needed.
6. Mark race results as Win or Loss in the table.
7. Use **Export progress** to back up to a JSON file.

---

## How to Use

### Select or Add an Umamusume

* Use the **Umamusume** dropdown in the header.
* Choose an existing entry or select **+ Add new** to create one.
* All edits save to the current Umamusume only.

### Edit Umamusume Details

Open the **Edit Umamusume Details** dialog to manage:

* **Photo**
  Upload JPG or PNG. The photo is embedded as a Data URL and saved immediately. Click the photo to enlarge it. Use **Remove photo** to clear it.

* **Track and Distance Aptitudes**
  Set ranks A to G for Turf, Dirt, Sprint, Mile, Medium, Long.

* **Pace Aptitude**
  Set ranks for Front runner, Pace chaser, Late surger, End closer. You can indicate strength in two paces by using high letters on both.

* **Support Cards**

  * **Recommended**: exactly 6 rows. Types are fixed to `SPD`, `STAM`, `PWR`, `GUTS`, `WIT`. A warning appears if there are not 6 entries.
  * **Alternative**: add or remove rows as needed.

* **Recommended Skills**
  Add or remove skill rows. The layout keeps the add button clear of inputs.

* **Support Deck Notes**
  Free text for composition notes and reasoning.

Click **Save** to apply changes.

### Views

* **All races**
  Full race list with filters and sorting.

* **Recommended**
  Subset where Track and Distance aptitudes are strong for this Umamusume. A race is recommended if Track and Distance ranks are `S` or `A`. Pace is not used in this filter.

* **Career**
  Shows races you have added to the Umamusume’s Career list. You can remove races right from the table.

* **Calendar**
  Month-grouped display. Click a race title to open a popover with actions.

### Filters and Search

Use the **Filters** toolbox to narrow by:

* Grade: G1, G2, G3
* Track: Turf, Dirt
* Distance: Sprint, Mile, Medium, Long
* Year: Junior, Classic, Senior, EX

Use **Quick search** to match race name, month, year, grade, distance, or track.

Click **Clear filters** to reset everything and the sort order.

### Mark Results

In the **Races** table, set **Win** or **Loss** per race. The row color and the stats cards update automatically. Use **Reset marks** to clear all results for the selected Umamusume.

### Career Tools and Crown Titles

* **Add to Career** from the table action button, the calendar popover, or manage in the **Career races** dialog.
* **Crown Titles**: toggle a full set at once. Each Crown toggles a list of target races. When enabled, those races are added to the **focus** set and are highlighted in the table. Partial selection shows an indeterminate checkbox state.

### Calendar and Popover

* Switch **View → Calendar**.
* Races are grouped by month.
* Click a race title to open a small popover with race info and a primary action to add or remove it from Career.

### Import, Export, and Reset

* **Export progress** writes a JSON file containing all Umamusume entries and race results.
* **Import progress** reads a compatible JSON and replaces the stored data.
* **Clear selected** wipes the current Umamusume’s data only.
* **Reset marks** clears all Win and Loss marks for the current Umamusume.

---

## Data Model

Each Umamusume is stored under its name. The structure uses a results map and an `info` object.

```json
{
  "Uma Name": {
    "results": {
      "Year__Date__Name": "win",
      "Year__Date__Name": "loss"
    },
    "tags": "",
    "info": {
      "photo": "data:image/...base64..." ,
      "careerKeys": ["Year__Date__Name"],
      "track": { "turf": "A", "dirt": "A" },
      "distance": { "sprint": "A", "mile": "A", "medium": "A", "long": "A" },
      "pace": { "front": "A", "chaser": "A", "late": "A", "end": "A" },
      "supports": [{ "type": "SPD", "name": "Card Name" }],
      "altSupports": [{ "type": "STAM", "name": "Card Name" }],
      "deckNote": "text",
      "skills": ["Skill Name"],
      "focusKeys": ["Year__Date__Name"]
    }
  }
}
```

### Local Storage Keys

* `umamusume.umalist`
  Main data blob containing all Umamusume.

* `umamusume.migr.clearCrowns.v2`
  One-time migration flag that cleared legacy `focusKeys` on load.

### Race Key Format

Keys are built as:

```
<Year>__<Date>__<Name>
```

Example:

```
Classic__Late May__Japanese Derby (Tokyo Yushun)
```

### Races and Crowns

* **RACES**
  Hardcoded array of races across **Junior**, **Classic**, **Senior**, and **EX**. Each item includes:

  * `date`: e.g., `Early Mar`, `Late Oct`, or `Scenario` for EX
  * `year`: Junior, Classic, Senior, EX
  * `name`: race title
  * `grade`: G1, G2, G3, or EX
  * `distance`: Sprint, Mile, Medium, Long
  * `track`: Turf or Dirt

* **CROWNS**
  Predefined Crown title sets. Each entry has:

  * `id`
  * `title`
  * `keys`: list of race keys that define the Crown set

---

## Customization

* **Add or edit races**
  Update the `RACES` array in the script block. Keep `name`, `date`, `year`, `grade`, `distance`, and `track` fields consistent.

* **Add or edit Crown Titles**
  Update the `CROWNS` array. Use exact race keys from the `RACES` list.

* **Change default Umamusume list**
  Edit `DEFAULT_UMA`. New names will appear after a fresh load or once you clear storage.

* **Recommended filter logic**
  The **Recommended** view currently includes races where Track and Distance ranks are `S` or `A`. Modify `recommendedMatches` if you want different rules.

---

## Styling

* The palette is defined on `:root` with CSS variables for background, panels, lines, text, accents, warnings, and row states.
* Table headers are sticky. Rows use accent borders for Win, Loss, and Focus.
* Layout uses responsive CSS grids.
* Dialogs are native `<dialog>` elements with custom styling.
* The skills editor uses a flex layout so the **+ Add skill** button never overlaps inputs.

---

## Privacy and Security

* All data stays in the browser in `localStorage`.
* No network calls.
* Photos are saved as Data URLs inside the stored object.
* Use **Export** to back up your data to a JSON file. Store it securely.

---

## Browser Support

* Designed for modern desktop browsers with support for:

  * `localStorage`
  * `<dialog>`
  * CSS Grid
  * FileReader and Data URLs

If your browser does not support `<dialog>`, the code falls back to opening the dialogs by setting the `open` attribute.

---

## Known Limitations

* Race list is hardcoded. You must edit the script to add or rename races.
* Recommended view does not consider Pace Aptitude.
* No multi-device sync. Use Export and Import to move data.
* No conflict resolution on Import. The imported file replaces the stored list.

---

## Changelog

* **v0.36**

  * Added Pace Aptitude section: Front runner, Pace chaser, Late surger, End closer.
  * Fixed skills editor layout so the add button does not overlap inputs.
  * Improved Crown Titles UI and focus highlighting.
  * Export JSON now includes `version: 36`.

---

## License
