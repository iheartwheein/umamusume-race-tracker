# Umamusume Race Tracker

Single-file web app to plan and track Umamusume races. Open the HTML file in any modern browser. All data is saved to `localStorage`.

---

## Table of contents

* [Quick start](#quick-start)
* [Features](#features)

  * [Top bar](#top-bar)
  * [Dashboard filters](#dashboard-filters)
  * [Stats](#stats)
  * [Races table](#races-table)
  * [Recommendation editors](#recommendation-editors)
  * [Crown Titles](#crown-titles)
  * [Umamusume Details panel](#umamusume-details-panel)
  * [Data management](#data-management)
  * [Race data](#race-data)
  * [Theming](#theming)
  * [Offline and privacy](#offline-and-privacy)
* [Common tasks](#common-tasks)
* [Data model](#data-model)
* [Sorting logic](#sorting-logic)
* [Known limits](#known-limits)
* [Troubleshooting](#troubleshooting)
* [Development notes](#development-notes)

---

## Quick start

1. Open `umamusume-race-tracker.html` in a browser.
2. Pick an Umamusume from the selector. Use “+ Add new” to create one.
3. Choose a view: **Recommended**, **Mandatory**, or **All races**.
4. Mark each race as **Win** or **Loss**.
5. Use **Edit Umamusume Details**, **Edit Recommended**, or **Edit Mandatory** to update data.

---

## Features

### Top bar

* Umamusume selector with “+ Add new”.
* View tabs: **Recommended**, **Mandatory**, **All races**.
* Quick Search across race name, month, and year.
* **Clear Filters** resets search and dashboard filters.

### Dashboard filters

Checkbox filters support multi-select and match the editors.

* Grades: G1, G2, G3
* Surface: Turf, Dirt
* Distance: Sprint, Mile, Medium, Long
* Year: Junior, Classic, Senior
* Participation: **All Participated Races** shows only races marked Win or Loss
* **Clear Tag Filter** turns off any tag-style filtering

### Stats

* **Total Races**, **Wins**, **Losses** update based on current view and filters.

### Races table

* Columns: Date, Race, Details `(Grade, Distance, Surface)`, Year, Result
* Sorting

  * Click **Race** to sort alphabetically. Click again to toggle direction.
  * Click **Year** to sort Junior → Classic → Senior, then by calendar order. Click again to toggle direction.
* Mark results

  * Dropdown: Not set, Win, Loss
  * Row highlight: gold for Win, red for Loss
* Focus highlight

  * Races tied to checked Crown Titles are tinted
* Add to lists from All races

  * In **All races** view, each row shows **+ Rec** and **+ Mand** to add that race to Recommended or Mandatory

### Recommendation editors

* **Edit Recommended** and **Edit Mandatory**
* Filters: Year, Grades, Distance, Surface, plus text search
* Two panes: **Matches** and **Selected**
* Add and Remove buttons
* Save stores selected races as exact keys `Year + Date + Name`

### Crown Titles

Checking a title highlights all target races on the dashboard.

* Classic Triple Crown: Satsuki Sho, Tokyo Yushun (Derby), Kikuka Sho
* Triple Tiara: Oka Sho, Japanese Oaks, Shuka Sho
* Senior Spring Triple Crown: Osaka Hai, Tenno Sho (Spring), Takarazuka Kinen
* Senior Autumn Triple Crown: Tenno Sho (Autumn), Japan Cup, Arima Kinen
* Dual Grand Prix: Takarazuka Kinen, Arima Kinen
* Dual Miles: Yasuda Kinen, Mile Championship
* Dual Sprints: Takamatsunomiya Kinen, Sprinters Stakes
* Dual Dirts: February Stakes, Champions Cup

Partial completion shows an indeterminate checkbox.

### Umamusume Details panel

* Photo

  * Displays the saved image. Click to enlarge.
  * Upload and removal are inside **Edit Umamusume Details**.
* Pace Aptitude

  * Multiple allowed
  * Labels: Front Runner, Pace Chaser, Late Surger, End Closer
* Track Aptitude

  * Turf rank and Dirt rank (S to G)
* Distance Aptitude

  * Sprint, Mile, Medium, Long ranks (S to G)
* Recommended Support Cards

  * Exactly 6 lines
  * Strict abbreviations only: `SPD`, `STAM`, `PWR`, `GUTS`, `WIT`
  * Example: `SPD: Kitasan Black`
  * Warning appears if not exactly six
* Alternative Support Cards

  * Any number
  * Same abbreviation rule
* Recommended Skills

  * Free list, global English names
* Support Deck Notes

  * Freeform notes
* **Edit Umamusume Details** dialog

  * Photo upload and removal
  * Multi-select pace aptitude
  * Rank pickers for track and distance
  * Six fixed rows for Recommended Support Cards
  * Add or remove Alternative Support Cards
  * Add or remove skills
  * Notes textarea
  * **Save** writes to storage
  * **Close** asks to confirm discard if there are unsaved changes

### Data management

* **Export Progress** downloads a JSON backup of all runners and marks.
* **Import Progress** loads a previous export.
* **Clear Selected** wipes data for the current runner only.
* All state persists to `localStorage`.

### Race data

* Curated catalogs for Junior, Classic, and Senior seasons.
* Each race has `date`, `year`, `name`, `grade`, `distance`, `surface`.
* Dates sort by month and by Early or Mid or Late for calendar order.

### Theming

* Theme uses CSS variables in `:root`.

  * `--bg`, `--panel`, `--soft`, `--text`, `--muted`, `--acc`, `--line`
  * Row highlights: `--winRow`, `--lossRow`, `--focusRow`, and their borders
* To change the palette, replace the `:root` block. No JavaScript changes required.

### Offline and privacy

* Runs locally in the browser
* No network calls
* Data stays in your browser unless you export it

---

## Common tasks

**Mark results**

1. Pick a view and set filters.
2. In the table, choose Win or Loss. Row color and counters update.

**Build Recommended or Mandatory**

1. Click Edit Recommended or Edit Mandatory.
2. Filter, Add races, Save.

**Add a single race from All races**

1. Switch to All races.
2. Click **+ Rec** or **+ Mand** on the row.

**Highlight Crown targets**

1. In Crown Titles, check a title.
2. Target races are tinted in the table.

**Show only races you ran**

1. Check **All Participated Races**.

**Edit Umamusume details**

1. Click **Edit Umamusume Details**.
2. Save to keep changes. Close to discard. A confirm dialog appears if there are unsaved changes.

**Export or Import**

* Export creates a backup JSON.
* Import restores from a backup JSON.

---

## Data model

Stored under key `umamusume.umalist` in `localStorage`.

```json
{
  "Runner Name": {
    "results": {
      "Senior__Late Nov__Japan Cup": "win",
      "Classic__Late Oct__Kikuka Sho": "loss"
    },
    "tags": "",
    "recommended": [],
    "info": {
      "photo": "data:image/png;base64,...",
      "pace": ["Pace Chaser", "Late Surger"],
      "track": { "turf": "A", "dirt": "G" },
      "distance": { "sprint": "A", "mile": "B", "medium": "A", "long": "A" },
      "supports": [
        "SPD: Kitasan Black",
        "STAM: Mejiro McQueen",
        "PWR: Narita Brian",
        "GUTS: ...",
        "WIT: ...",
        "SPD: ..."
      ],
      "altSupports": ["WIT: Fine Motion"],
      "skills": ["Corner Recovery", "Tempo Up"],
      "deckNote": "Notes here",
      "recommendedKeys": ["Classic__Early Apr__Satsuki Sho"],
      "mandatoryKeys": ["Senior__Late Nov__Japan Cup"],
      "focusKeys": ["Senior__Late Nov__Japan Cup"]
    }
  }
}
```

Race key format: `Year__Date__Name`.

---

## Sorting logic

* Year order: Junior, then Classic, then Senior.
* Inside each year: calendar order by month and by Early or Mid or Late.
* Race sorting: Unicode string compare by race name.
* If the filtered set contains only one year and sort is set to Year, the code resets to default calendar sort.

---

## Known limits

* Data is local to the browser and profile.
* Clearing site data removes stored progress.
* Photos are stored as base64 inside `localStorage`.
* Race catalog is fixed in the file.

---

## Troubleshooting

* Save in Edit Umamusume Details does nothing
  Make sure the browser allows `localStorage`.

* Photo does not appear
  Use JPG or PNG. Upload from the editor, then Save.

* Import fails
  Import a JSON exported by this app.

* Table is empty
  Clear filters, clear search, and uncheck All Participated Races.

---

## Development notes

* Single HTML file. No build step.
* Styles are in a `<style>` tag. Logic is in a `<script>` tag.
* Race catalogs live in `RACES_JUNIOR`, `RACES_CLASSIC`, and `RACES_SENIOR`, combined into `RACES`.
* To change the theme, edit the `:root` variables only.
