# Habit Tracker

A personal daily habit tracker that runs entirely in your browser. No server, no accounts, no internet required after you open the file.

## How to open it

1. Open the `habit-tracker/` folder on your laptop.
2. Double-click `index.html` — it will open in your default browser.
3. Bookmark it for fast access, or drag the file onto your bookmark bar.

That's it. Your data is saved automatically in the browser's `localStorage` and persists between sessions, even if you close the tab or restart your computer.

> **Tip:** Because data lives in the browser, it is tied to the specific browser you use. If you switch browsers or clear site data, use the Export backup feature first.

---

## Habits tracked

| Habit | Type | Goal |
|-------|------|------|
| Gym | Checkbox | Done / not done |
| Meditation | Checkbox | Done / not done |
| No Junk Food | Checkbox | Done / not done |
| Water | Number | 8 glasses/day |
| Reading | Number | 20 minutes/day |

---

## Features

- **Today tab** — check off habits and log numbers for the current day. A progress bar shows how many of your 5 habits are complete.
- **Streaks** — each habit shows your current streak (consecutive days you hit the goal). Streaks starting from yesterday are safe — an incomplete *today* does not break your streak.
- **Calendar tab** — monthly grid. Green = all 5 habits done. Yellow = partial. Gray = nothing logged. Tap any past day to edit it.
- **Stats tab** — longest streaks, water and reading totals for the week and month, and a monthly completion percentage.
- **Data tab** — export a JSON backup, import a backup, or browse all your logged days in a readable text format.

---

## Backing up and restoring

### Export
Go to **Data → Export Backup**. A file named `habit-tracker-YYYY-MM-DD.json` will download.

### Import
Go to **Data → Import Backup** and select a previously exported file. This overwrites current data, so export first if you have unsaved changes.

---

## Opening on your phone

1. Make sure your laptop and phone are on the same Wi-Fi network.
2. Note your laptop's local IP address (e.g. `192.168.1.42`).
3. Serve the file with Python:
   ```
   cd habit-tracker
   python3 -m http.server 8080
   ```
4. On your phone, open `http://192.168.1.42:8080`.

Alternatively, copy the `index.html` file to your phone and open it with a file manager that can open HTML files (e.g., the Files app on iOS with a browser, or any file manager on Android).

---

## Adding a new habit later

Open `index.html` in a text editor and find the `HABITS` array near the top of the `<script>` block (search for `const HABITS`):

```js
const HABITS = [
  { key:'gym',        name:'Gym',          icon:'🏋️', type:'bool' },
  { key:'meditation', name:'Meditation',   icon:'🧘', type:'bool' },
  { key:'noJunkFood', name:'No Junk Food', icon:'🥗', type:'bool' },
  { key:'water',      name:'Water',        icon:'💧', type:'num', unit:'glasses', goal:8  },
  { key:'reading',    name:'Reading',      icon:'📚', type:'num', unit:'min',     goal:20 },
];
```

### Adding a checkbox habit

Append a line like this:

```js
{ key:'coldShower', name:'Cold Shower', icon:'🚿', type:'bool' },
```

### Adding a number habit

```js
{ key:'steps', name:'Steps', icon:'🚶', type:'num', unit:'k steps', goal:10 },
```

- `key` — a unique camelCase identifier (used for storage, no spaces).
- `name` — display name.
- `icon` — any single emoji.
- `type` — `'bool'` for checkbox, `'num'` for a number with +/− buttons.
- `unit` — label shown next to the number (number habits only).
- `goal` — the daily target that counts as "complete" for streak purposes (number habits only).

Save the file and refresh the browser. The new habit appears immediately. Old data is unaffected.

---

## Data format

All data is stored under the `localStorage` key `habitTracker_v1` as a JSON object keyed by date string:

```json
{
  "2026-05-07": {
    "gym": true,
    "meditation": false,
    "noJunkFood": true,
    "water": 9,
    "reading": 35
  }
}
```

This is also the format of the exported backup file.
