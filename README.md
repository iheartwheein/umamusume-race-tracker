# Umamusume Race Tracker

A single-file web app for tracking races, planning routes for each Umamusume, and keeping per-character progress. It runs in the browser, stores data in `localStorage`, and requires no build step.

---

## Features

### Views

* **Recommended**
  Shows races that match the selected Umamusume’s **track** and **distance** aptitudes. Only ranks **S** or **A** are considered a match. Manual picks you saved as “recommended” are included too.

* **Mandatory**
  Shows only the races you explicitly saved as “mandatory” for the selected Umamusume.

* **All races**
  Full catalog. From here you can add races to “recommended” or “mandatory”.

* **Calendar**
  Month grid across the year. Races are grouped by month. Click a race title to add it to “recommended” or “mandatory”.

All four views share the same filter panel and the same default sort order.

---

### Default sorting and manual sorting

* **Default**: always sorted **by year** in this order **Junior → Classic → Senior**, then by **calendar order** (month Early → Mid → Late, then name).
* You can click **Race** or **Year** in the table header to sort. Clicking the **Year** header toggles ascending or descending. Clicking **Race** sorts by name and toggles direction.

Calendar view uses the same year-first order inside each month.

---

### Filters

Found under **Dashboard → Filters**:

* **View**: Recommended, Mandatory, All races, Calendar
* **Grades**: G1, G2, G3
* **Track**: Turf, Dirt
* **Distance**: Sprint, Mile, Medium, Long
* **Year**: Junior, Classic, Senior
* **Participation**: Show only races you already marked with a Win or Loss

You can also **quick search** by race name, month keyword, or year.

**Note**: “Clear filters” resets all filter checkboxes and search, and restores the default sort.

---

### Stats widget

* **Total races** currently shown
* **Wins** and **Losses** counted from your marks
* Action: **Reset marks** to clear all Win/Loss results for the current Umamusume

---

### Crown titles

Choose crown title presets to highlight focus races:

* Classic Triple Crown
* Triple Tiara
* Senior Spring Triple Crown
* Senior Autumn Triple Crown
* Dual Grand Prix
* Dual Miles
* Dual Sprints
* Dual Dirts

Each crown title is a checkbox. When checked, all its races are added to the **focus** set and are highlighted in the table and calendar. Unchecking removes them from focus. Partial selection shows the indeterminate state.

---

### Per-Umamusume details

Open **Edit Umamusume details** to configure:

* **Photo**
  Stored as a data URL in `localStorage`. Click the photo to open the lightbox. You can remove it.

* **Pace aptitude**
  Multi-select. Saved for reference. Not used for filtering.

* **Track aptitude**
  Turf rank and Dirt rank. Ranks are S to G.

* **Distance aptitude**
  Sprint, Mile, Medium, Long ranks. Ranks are S to G.

* **Recommended support cards**
  Exactly six rows. Type codes: SPD, STAM, PWR, GUTS, WIT.

* **Alternative support cards**
  Any number of rows with a remove button.

* **Recommended skills**
  Any number of rows with a remove button.

* **Support deck notes**
  Freeform text.

Your edits save when you click **Save**. If you cancel, you can discard changes.

---

### Recommended logic

Recommended view shows a **union** of:

1. All races where:

   * The race **track** matches a track aptitude with rank **S** or **A**
     Turf race requires Turf S or A.
     Dirt race requires Dirt S or A.
   * The race **distance** matches a distance aptitude with rank **S** or **A**
     Uses the distance bucket name: `sprint`, `mile`, `medium`, `long`.

2. Any races you manually added to the recommended list.

Pace aptitude does not affect recommended filtering.

---

### Marking results

In the table, choose **Win** or **Loss** per race. These marks are saved per Umamusume. “All participated races” filter uses these marks to show a history view.

---

### Add races to lists

* **All races** table: each row has **+ Rec** and **+ Mand** buttons
* **Calendar** view: click a race title to open a small dialog and choose the list

---

### Data management

* **Export progress**
  Downloads a JSON file with schema version and all per-Umamusume data.

* **Import progress**
  Upload a previously saved JSON file to restore your data.

* **Clear selected**
  Wipes all data for the currently selected Umamusume.

All data is stored under the `localStorage` key `umamusume.umalist` in the browser.

---

## Quick start

1. Download the single HTML file from the repository.
2. Open it in any modern desktop browser.
3. Use the **Umamusume** dropdown to pick a character or add a new one.
4. Open **Edit Umamusume details** and set:

   * Track aptitudes for Turf and Dirt
   * Distance aptitudes for Sprint, Mile, Medium, Long
   * Optional photo and notes
5. Switch to **Recommended** view to see aptitude-based suggestions.
6. Add races to **Mandatory** or **Recommended** as needed.
7. Mark **Win** or **Loss** as you play.
8. Export JSON for backups.

No server is required.

---

## Data model

### Storage layout

`localStorage["umamusume.umalist"]` holds an object keyed by Umamusume name:

```json
{
  "Special Week": {
    "results": { "Classic__Late May__Japanese Derby (Tokyo Yushun)": "win" },
    "tags": "",
    "recommended": [],
    "info": {
      "photo": "data:image/png;base64,...",
      "recommendedKeys": [],
      "mandatoryKeys": [],
      "pace": ["Front Runner", "Late Surger"],
      "track": { "turf": "A", "dirt": "B" },
      "distance": { "sprint": "C", "mile": "A", "medium": "S", "long": "B" },
      "supports": ["SPD: Example Card"],
      "altSupports": ["STAM: Another Card"],
      "deckNote": "Notes here",
      "skills": ["Skill A", "Skill B"],
      "focusKeys": ["Classic__Early Apr__Satsuki Sho"]
    }
  }
}
```

* `results`: map of `raceKey → "win" | "loss"`
* `info.recommendedKeys` and `info.mandatoryKeys`: arrays of race keys
* `info.focusKeys`: set by crown title checkboxes
* `track` and `distance`: ranks `S` to `G` as strings

### Race identity and keys

Every race has a unique key:

```
{year}__{date}__{name}
```

Example:

```
Classic__Late May__Japanese Derby (Tokyo Yushun)
```

Keys are used for recommended, mandatory, focus, and results.

---

## Races and calendar order

* Race lists are defined in constants:

  * `RACES_JUNIOR`
  * `RACES_CLASSIC`
  * `RACES_SENIOR`
  * Combined as `RACES`
* `date` uses `Early|Mid|Late` plus month name, for example `Early Apr`
* Calendar order:

  1. Year bucket: Junior → Classic → Senior
  2. Month order: Jan → … → Dec
  3. Position within month: Early → Mid → Late
  4. Name as tiebreaker

Helper functions:

* `parseDateStr("Early Apr")` → `{ m: 3, p: 1 }`
* `sortByCalendar(a, b)`
* `sortByYearThenCalendar(a, b, dir)`

---

## UI reference

### Header

* **Umamusume** selector
  Add new by choosing “+ Add new”
* **Quick search**
  Substring search across name, month, year, grade, distance, track
* **Clear filters**
  Resets filters and sort to defaults

### Dashboard

* **Filters** panel
  View, Grades, Track, Distance, Year, Participation
  “Clear tag filter” refreshes the list view

* **Recommendations** panel
  Edit Recommended and Edit Mandatory
  Crown titles list

* **Stats** panel
  Total, Wins, Losses, Reset marks

### Tables and calendar

* Table headers **Race** and **Year** are clickable for sorting
* **Result** column: Win or Loss dropdown per race
* **All races** only: “+ Rec” and “+ Mand” per row
* **Calendar**: click a race title to add it to a list

### Editors and dialogs

* **Edit recommended races** and **Edit mandatory races**
  Search, filter, add from matches into Selected, then Save

* **Edit Umamusume details**
  Photo, aptitudes, supports, skills, notes

* **Add race** dialog
  Choose to add to Recommended or Mandatory

* **Photo lightbox**
  Click photo to open. Close from dialog.

---

## How recommended is calculated

Function: `matchesAptitudes(r, info)`

* Check track:

  * Turf race requires `info.track.turf` in `{S, A}`
  * Dirt race requires `info.track.dirt` in `{S, A}`
* Check distance:

  * Race distance bucket requires the same bucket in `info.distance` to be `{S, A}`

Function: `buildRecommendedBase(info)`

* Starts with all races where `matchesAptitudes` is true
* Adds any races saved in `info.recommendedKeys`
* The filter panel and search still apply afterward
* Sorted by year then calendar by default

---

## Export and import

* **Export progress** creates `umamusume_race_progress.json`
  Contains `version` and `umaList`.

* **Import progress** accepts a file in that format
  Replaces existing `umalist` and reloads the app.

---

## Customization

* **Add a new Umamusume**
  Use the dropdown and select **+ Add new**

* **Edit races**
  Update `RACES_JUNIOR`, `RACES_CLASSIC`, `RACES_SENIOR`.
  Keep `year`, `date`, `name`, `grade`, `distance`, `track`.
  Race keys depend on the exact strings.

* **Add a crown title**
  Add to `CROWNS` using exact race keys.

* **Styling**
  Tweak CSS variables at the top of the file.

---

## Troubleshooting

* **Recommended is empty**
  Check the selected Umamusume’s aptitudes.
  Recommended only includes races where **both** track and distance are rank **S** or **A**.
  Also check if the filter panel hides them by grade or year.

* **Crown title highlights only some races**
  The app uses exact keys. If you rename races or dates, update the `CROWNS` entries to match the keys in `RACES`.

* **No photo shown**
  You must pick an image file. It saves automatically after selection.

* **Sorting does not look right**
  Click **Year** to toggle direction. Default is Year ascending. Calendar view uses the same internal ordering.

---

## Technical notes

* **No dependencies**. Single HTML file with inline CSS and JS.
* **Rendering model**. Declarative render driven by:

  * `state` object for global view and sorting
  * `umaList` in `localStorage` for persistent data
* **Key functions**:

  * `matchesAptitudes` controls recommended logic
  * `sortByYearThenCalendar` standardizes ordering
  * `buildRecommendedBase` merges aptitude picks with manual picks
  * `renderTable` and `renderCalendar` are exclusive per view
* **Accessibility**:

  * Table headers are clickable for sorting
  * Dialogs are native `<dialog>` elements

---

## License

MIT. Use and modify as you like.
