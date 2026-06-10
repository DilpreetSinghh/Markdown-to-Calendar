# Markdown & Calendar

> Convert time-blocked Obsidian daily notes into calendar events — and calendar events back into daily-note time blocks. 100% offline, right in your browser.

Free & open source. Found it useful? 🐙 Star it 🤩 on [GitHub](https://github.com/DilpreetSinghh/Markdown-to-Calendar) — it means a lot 😍

---

## What it does

`Markdown & Calendar` is a tiny local-only web app with two modes:

- **Markdown → Calendar**
  - Reads your Obsidian daily notes, finds time-blocked task lines, and exports them as an `.ics` file you can import into Google Calendar, Apple Calendar, Outlook, or any iCalendar-compatible app.
- **Calendar → Markdown**
  - Reads your `.ics` calendar files and turns events into Obsidian-style daily-note time blocks you can paste into your notes.

All parsing happens entirely in your browser. **No data ever leaves your device.**

---

## Live app

- **GitHub Pages:** https://dilpreetsinghh.github.io/Markdown-to-Calendar/

Open the link in any modern browser (desktop works best).

---

## How to use

### 1. Markdown → Calendar

1. Open the [live app](https://dilpreetsinghh.github.io/Markdown-to-Calendar/).
2. Make sure the mode switch at the top says **Markdown to Calendar**.
3. Click **Select Folder** or drag & drop your daily notes folder.
4. Optionally choose a **Calendar tagging** mode (`#tags` or `@labels`).
5. Click **Scan and Preview Events**.
6. Adjust tag settings if needed (enable/disable, rename calendars, strip tags from titles).
7. Choose output format and click **Download Calendar File**.
8. Import the `.ics` (or `.ical` / `.icalendar` / `.ifb`) file into your calendar app.

### 2. Calendar → Markdown

1. Open the [live app](https://dilpreetsinghh.github.io/Markdown-to-Calendar/).
2. Flip the mode switch to **Calendar to Markdown**.
3. Drag & drop one or more calendar files (`.ics`, `.ical`, `.icalendar`, `.ifb`) or click **Select Calendar Files**.
4. (Optional) Choose how to emit tags from `CATEGORIES` (`#tags`, `@labels`, or no tags).
5. Click **Convert to Markdown** to preview the generated daily-note blocks.
6. Click **Download Markdown** to save a `.md` file, or copy from the preview.

---

## Features

- **Two-way pipeline** between Obsidian daily notes and calendar files.
- **Select Folder** or **Drag & Drop** your Obsidian vault / daily notes folder.
- Recursively scans all subfolders.
- Detects time-blocked task lines like:
  ```markdown
  - [x] 0115-0140 Morning study block ✅ 2025-02-08
  - [ ] 0900-1700 Rest / Sleep
  ```
- Indented bullets under a task become the event **description**.
- Optional calendar tagging via `#tags` or `@labels`.
- Tag configuration table — rename, enable/disable each tag as a calendar.
- Toggle to strip calendar tags from exported event titles.
- Multiple export formats: `.ics`, `.ical`, `.icalendar`, `.ifb`.
- Calendar → Markdown mode: groups events by date and emits Obsidian-style checkboxes with `HHMM-HHMM` ranges.
- Works on Chrome, Edge, Brave, Firefox, Safari (11.1+).

---

## Daily note format

### Filename

```text
YYYY MMDD.md  →  e.g. 2026 0115.md, 2025 0208.md
```

### Task line format

```markdown
- [x] 0115-0140 Event title
- [ ] 0900-1700 Another event #work
- [x] 2030-2115 Study session @personal
    - Sub-point becomes event description
    - Another detail line
```

**Time format:** 3–4 digit compact (`0115` = 01:15 · `900` = 09:00 · `1700` = 17:00), interpreted as 24-hour time.

---

## Calendar tagging (optional)

| Mode | How it works |
|---|---|
| No explicit tags | All time-blocked tasks are exported |
| `#tags` | Extracts `#work`, `#personal`, `#cal/study` etc. from task lines |
| `@labels` | Extracts `@work`, `@personal` etc. from task lines |

After scanning, a tag configuration table appears — you can rename each tag's calendar name and choose which ones to include. Events with no selected tag are excluded when tagging is active.

On the calendar → markdown side, `CATEGORIES` values are turned back into `#tag` / `@label` strings (or skipped) depending on your output choice.

---

## Privacy

- Fully offline — no server, no network requests.
- Files are only read after you explicitly select or drop them.
- Nothing is uploaded anywhere; everything stays in your browser.

---

## Development

- Main entry: `obsidian-daily-notes-to-ics.html` (single-page app).
- Styles: `styles.css` (glassmorphism shell, iOS-like transitions, and controls).
- All logic is vanilla JavaScript — no frameworks.

To work locally:

1. Clone the repo.
2. Checkout the `Developer-Beta` branch if you want the latest UI / calendar↔markdown work.
3. Open `obsidian-daily-notes-to-ics.html` directly in your browser, or serve via a simple static server.

---

## Licence

[GNU Affero General Public License v3.0](./LICENSE)
