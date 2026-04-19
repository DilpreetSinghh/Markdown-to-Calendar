# Markdown to Calendar

> Convert time-blocked Obsidian daily notes into calendar events — 100% offline, right in your browser.

Free & open source. Found it useful? 🐙 Star it 🤩 on [GitHub](https://github.com/DilpreetSinghh/Markdown-to-Calendar) — it means a lot 😍

---

## What it does

Reads your Obsidian daily notes, finds time-blocked task lines, and exports them as an `.ics` file you can import into Google Calendar, Apple Calendar, Outlook, or any iCalendar-compatible app.

All parsing happens entirely in your browser. **No data ever leaves your device.**

---

## How to Use

1. Open the [live app](https://dilpreetsinghh.github.io/Markdown-to-Calendar/) *(requires GitHub Pages to be enabled)*
2. Click **Select Folder** or drag & drop your daily notes folder
3. Optionally choose a **Calendar tagging** mode (`#tags` or `@labels`)
4. Click **Scan and Preview Events**
5. Adjust tag settings if needed
6. Choose output format and click **Download Calendar File**
7. Import the `.ics` file into your calendar app

---

## Features

- **Select Folder** or **Drag & Drop** your Obsidian vault / daily notes folder
- Recursively scans all subfolders
- Detects time-blocked task lines like:
  ```markdown
  - [x] 0115-0140 Morning study block ✅ 2025-02-08
  - [ ] 1000-1700 Rest / Sleep
  ```
- Indented bullets under a task become the event **description**
- Optional calendar tagging via `#tags` or `@labels`
- Tag configuration table — rename, enable/disable each tag as a calendar
- Toggle to strip calendar tags from exported event titles
- Multiple export formats: `.ics`, `.ical`, `.icalendar`, `.ifb`
- Works on Chrome, Edge, Brave, Firefox, Safari (11.1+)

---

## Daily Note Format

### Filename

```
YYYY MMDD.md  →  e.g. 2026 0115.md, 2025 0208.md
```

### Task line format

```markdown
- [x] 0115-0140 Event title
- [ ] 900-1700 Another event #work
- [x] 2030-2115 Study session @personal
    - Sub-point becomes event description
    - Another detail line
```

**Time format:** 3–4 digit compact (`0115` = 01:15 · `900` = 09:00 · `1700` = 17:00), interpreted as 24-hour time.

---

## Calendar Tagging (Optional)

| Mode | How it works |
|---|---|
| No explicit tags | All time-blocked tasks are exported |
| `#tags` | Extracts `#work`, `#personal`, `#cal/study` etc. from task lines |
| `@labels` | Extracts `@work`, `@personal` etc. from task lines |

After scanning, a tag configuration table appears — you can rename each tag's calendar name and choose which ones to include. Events with no selected tag are excluded when tagging is active.

---

## Privacy

- Fully offline — no server, no network requests
- Files are only read after you explicitly select them
- Nothing is uploaded anywhere; everything stays in your browser

---

## Licence

[GNU Affero General Public License v3.0](./LICENSE)
