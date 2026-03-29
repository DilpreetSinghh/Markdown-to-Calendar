# Obsidian Daily Notes → iCalendar

A small local-only web app that converts time‑blocked tasks in your Obsidian daily notes into an iCalendar file you can import into Google Calendar, Outlook, Apple Calendar, and similar tools.

All parsing and `.ics` generation happens entirely in your browser. Your Obsidian vault never leaves your machine.

---

## Features

- Reads Obsidian daily notes from a chosen folder **and all its subfolders**.
- Supports daily note filenames of the form `YYYY MMDD.md`, for example `2026 0115.md`.
- Detects time‑blocked tasks such as:

  ```markdown
  - [x] 0115- 0140 Morning study block ✅ 2025-02-08
  - [ ] 1000- 1700 Rest / Sleep
  ```

- Includes both completed and incomplete tasks as events.
- Treats **indented bullets under a task** as the event description:

  ```markdown
  - [x] 2030 - 2115 Evening study session ✅
      - Topic: Upcoming exam preparation
      - Duration: 01 Jan 2025 to 28 Feb 2025
      - Access valid until: 31 Dec 2025
      - Write summary notes
      - Plan next topics
  ```

- Optional **calendar tagging**:
  - Use `#tags` like `#work`, `#personal`, `#cal/selfstudy`.
  - Or use `@labels` like `@work`, `@personal`.
  - A configuration table lets you choose which tags become calendars and what name each calendar should have.
  - Events with no selected calendar tag can be excluded when tagging mode is enabled.
  - Each exported event can include an iCalendar `CATEGORIES:` property with the configured calendar name.

- **Title clean‑up** toggle:
  - Checkbox: “Remove selected calendar tags from event titles”.
  - When enabled (default), calendar tags like `#cal/work` or `@Work` are removed from the exported event titles while still being used for categories.

- **Tagging summary** after each scan:
  - Shows the number of events and distinct files for each discovered tag, for example:

    ```text
    Tagging summary: #cal/work – 30 events in 5 files; #cal/personal – 25 events in 6 files
    ```

- **Multiple output formats** for the same calendar content:
  - `.ics` (default)
  - `.ical`
  - `.icalendar`
  - `.ifb` (free/busy)

---

## Requirements

- Recent desktop Chrome, Edge, Brave, or another Chromium‑based browser that supports the File System Access API.
- Must be served from `http://localhost` or `https://` (secure context). Opening the file directly with `file:///` will not work.
- Any simple static HTTP server (for example Python 3’s `http.server`).

---

## Folder and note format

### Folder structure

Pick the root folder that contains your daily notes. The app will recursively scan all subfolders, for example:

```text
ObsidianVault/
  Daily/
    2026/
      2026 01/
        2026 0115.md
        2026 0116.md
      2026 02/
        2026 0208.md
```

### Filename format

Daily note filenames must match the pattern:

```text
YYYY MMDD.md
```

Examples:

- `2026 0115.md`
- `2025 0208.md`

The script infers the calendar date from the filename.

### Time‑blocked task format

The app looks for task lines like:

```markdown
- [x] 0115- 0140 Morning study block ✅ 2025-02-08
- [ ] 1000- 1700 Rest / Sleep
```

Rules:

- A checkbox (`[x]` or `[ ]`).
- A start and end time in **3‑ or 4‑digit compact format**:
  - `0115` → 01:15
  - `900`  → 09:00
  - `1700` → 17:00
- Times are interpreted as 24‑hour times on the date from the filename.
- Everything after the time range is treated as the event summary.

### Description from indented bullets

Any **indented lines** that immediately follow the task are attached as the event description. For example:

```markdown
- [x] 2030 - 2115 Evening study session ✅
    - Topic: Upcoming exam preparation
    - Duration: 01 Jan 2025 to 28 Feb 2025
    - Access valid until: 31 Dec 2025
    - Write summary notes
    - Plan next topics
```

produces:

- `SUMMARY`: `Evening study session ✅`
- `DESCRIPTION`:

  ```text
  Topic: Upcoming exam preparation
  Duration: 01 Jan 2025 to 28 Feb 2025
  Access valid until: 31 Dec 2025
  Write summary notes
  Plan next topics
  ```

Indentation stops when a blank line, a new top‑level task, or a non‑indented line is reached.

---

## Calendar tagging (optional)

Use tags or labels to decide which tasks become events and how they are grouped.

### Tagging modes

The **Calendar tagging** dropdown has three modes:

- `No explicit calendar tags` (default)
  - All detected time‑blocked tasks are exported.
- `Use #tags (e.g. #work, #personal)`
  - The app extracts `#something` tokens from the task line.
- `Use @labels (e.g. @work, @personal)`
  - The app extracts `@something` tokens from the task line.

When tag mode is `#` or `@`:

- After scanning, a **Calendar tags detected** table appears.
- For each discovered tag you can:
  - Tick whether it should be treated as a calendar.
  - Edit the human‑readable calendar name (for example `#cal/selfstudy` → `Self Study`).
- Only events containing at least one **selected** calendar tag are exported.
- Each exported event can include a `CATEGORIES:CalendarName` line.

### Stripping tags from titles

Below the tag table there is a checkbox:

> Remove selected calendar tags from event titles

- When checked (default), any tag used as a calendar is removed from the event title before export.
- When unchecked, titles are exported exactly as they appear in your notes.

### Tagging summary

After each scan, if tag mode is active and tags are present, the status area shows an additional line such as:

```text
Tagging summary: #cal/work – 30 events in 5 files; #cal/personal – 25 events in 6 files
```

This is based on all parsed events and shows how many events and distinct files each tag touches.

### Rescanning after tag‑mode changes

If you change the **Calendar tagging** mode after scanning (for example from `No explicit calendar tags` to `Use #tags`):

- The app marks that a rescan is required.
- Status explains that you should click **“Scan & preview events”** again.
- Download is disabled until you rescan.
- Attempting to download before rescanning shows a warning.

This ensures the preview, tag configuration, and export are always consistent with the current tagging mode.

---

## Running locally

1. Place `obsidian-daily-notes-to-ics.html` in a folder on your machine.
2. Start a local HTTP server in that folder. For example with Python 3:

   ```bash
   python -m http.server 8000 --bind 127.0.0.1
   ```

3. Open the app in your browser at:

   ```text
   http://localhost:8000/obsidian-daily-notes-to-ics.html
   ```

4. Use the interface:

   1. **Pick daily‑notes folder** – choose the root containing your daily notes.
   2. Optionally choose a **Calendar tagging** mode.
   3. Click **Scan & preview events**.
   4. (If using tags) adjust the tag table and the “Remove selected calendar tags from event titles” checkbox.
   5. Choose an output **Format**.
   6. Click **Download** to export the calendar file.

5. Import the exported calendar file into your calendar application.

---

## Privacy

- All parsing and ICS generation happens in your browser.
- Files are only read after you explicitly choose a folder.
- No data is uploaded anywhere; `localhost` traffic never leaves your machine.

---

## Limitations

- Only supports the filename and task formats described above.
- Only time‑blocked tasks with a checkbox and start–end time range are converted into events.
- Tagging is based on tags on the task line itself, not YAML frontmatter or note‑level tags.
- Designed for personal use; there is no multi‑user or server‑side component.

---

## Licence

Add your preferred licence (for example MIT).
