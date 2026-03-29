# Obsidian Daily Notes → ICS

A tiny local-only web app that converts time-blocked tasks in your Obsidian daily notes into an `.ics` calendar file you can import into Google Calendar, Outlook, Apple Calendar, etc.

All parsing and ICS generation happens **entirely in your browser**. Your Obsidian vault never leaves your machine.

---

## Features

- Reads Obsidian daily notes from a folder (and all subfolders).
- Supports filenames like `YYYY MMDD.md` (for example `2026 0115.md`).
- Detects time-blocked tasks such as:

  ```markdown
  - [x] 0115- 0140 Analysis of Course on PW ✅ 2025-02-08
  - [ ] 1000- 1700 Sleep
  ```

- Includes **both completed and incomplete** tasks as events.
- Treats **indented bullets under a task** as the event description:

  ```markdown
  - [x] 2030 - 2115 Analysis of Course on PW ✅ 2025-02-08
      - SBI CLERK CRASH COURSE (PRE+MAINS) 2024-25
      - Course Duration: 10 Jan 2025 to 28 Feb 2025
      - Validity: 31 Dec 2025
  ```

  becomes an event with a description containing each bullet on its own line.

- Generates a single `.ics` file from all matching notes.

---

## Requirements

- **Browser**: Recent desktop version of Chrome, Edge, Brave, or another Chromium‑based browser that supports the File System Access API. Firefox and Safari are not supported.  
- **Context**: Must be served from `http://localhost` or `https://` (secure context). Direct `file:///` opening will not work because the API is disabled there.[web:23][web:45]
- **Python 3** (or any simple HTTP server) for local-only hosting.

---

## Folder and note format

### Folder structure

You normally pick a **root folder** that contains your yearly/monthly/daily structure, for example:

```text
ObsidianVault/
  Personal/
    Calendar/
      Daily/
        2026/
          2026 01/
            2026 0115.md
            2026 0116.md
          2026 02/
            2026 0208.md
```

The app will **recursively** search all subfolders under the folder you pick.

### Filename format

Daily note filenames must follow:

```text
YYYY MMDD.md
```

Examples:

- `2026 0115.md`
- `2025 0208.md`

The script infers the calendar date from this pattern.

### Time-blocked task format

The app looks for task lines like:

```markdown
- [x] 0115- 0140 Analysis of Course on PW ✅ 2025-02-08
- [ ] 1000- 1700 Sleep
```

Rules:

- A checkbox (`[x]` or `[ ]`) – both completed and incomplete tasks are included.
- A start time and end time, in **3- or 4‑digit compact format**:

  - `0115` → 01:15  
  - `900`  → 09:00  
  - `1700` → 17:00  

- Times are interpreted as 24‑hour times on the date from the filename.

Everything after the time range is treated as the event summary.

### Description from indented bullets

Any **indented lines** immediately following the task are attached as the event description. For example:

```markdown
- [x] 2030 - 2115 Analysis of Course on PW ✅ 2025-02-08
    - SBI CLERK CRASH COURSE (PRE+MAINS) 2024-25
    - Course Duration: 10 Jan 2025 to 28 Feb 2025
    - Validity: 31 Dec 2025
    - Writing total content of course
    - Which Subjects to do?
```

becomes:

- `SUMMARY`: `Analysis of Course on PW ✅ 2025-02-08`
- `DESCRIPTION`:

  ```text
  SBI CLERK CRASH COURSE (PRE+MAINS) 2024-25
  Course Duration: 10 Jan 2025 to 28 Feb 2025
  Validity: 31 Dec 2025
  Writing total content of course
  Which Subjects to do?
  ```

Indented text stops when:

- A blank line is reached, or
- The next top‑level task `- [ ]` / `- [x]` appears, or
- A non‑indented, non‑task line appears (to avoid accidentally swallowing headings).

---

## How to run locally (no internet / local-only)

1. **Download** `obsidian-daily-notes-to-ics.html` from this repository.

2. **Place it in a folder**, for example:

   ```text
   C:\obsidian-ics-tool\
     obsidian-daily-notes-to-ics.html
   ```

3. **Start a local HTTP server** in that folder.

   With Python 3:

   ```bash
   cd C:\obsidian-ics-tool
   python -m http.server 8000 --bind 127.0.0.1
   ```

   This serves the folder at `http://localhost:8000` and is only accessible on your machine.

4. **Open the app** in your browser:

   - Go to: `http://localhost:8000/obsidian-daily-notes-to-ics.html`

5. **Use the UI**:

   - Click **“1. Pick daily-notes folder”**  
     - Choose the root folder that contains your daily notes (e.g. `Daily/` or `2026/`).  
     - The browser shows a native folder picker and only grants access to the folder you choose.

   - Click **“2. Scan & preview events”**  
     - The app recursively scans all `.md` files under that folder.  
     - It displays a list of found events (date, time range, summary, source filename).

   - Click **“3. Download .ics”**  
     - A single `.ics` file (e.g. `obsidian-daily-notes-20260329.ics`) is generated and downloaded.

6. **Import the `.ics` file** into your calendar app (Google Calendar, Outlook, Apple Calendar, etc.).

---

## Privacy

- The app runs entirely in your browser.
- Files are read via the browser’s File System Access API only after you explicitly choose a folder.
- No data is sent to any server; `localhost` traffic never leaves your machine.[web:23][web:45]

---

## Limitations and notes

- Only supports the filename and task formats described above.
- Only time-blocked lines with a checkbox and a start–end time range are converted into events.
- Designed for personal workflows; no authentication, multi‑user support, or backend.
- Requires a Chromium‑based browser with File System Access support; Firefox/Safari users need an alternative approach.

---

## Licence

Add your preferred licence here (for example MIT).
