# Habit Tracker

A personal daily habit tracker that runs entirely in your browser. No server, no accounts, no internet required after you open the file (charts need internet to load Chart.js from CDN on first use).

## How to open it

1. Open the `habit-tracker/` folder on your laptop.
2. Double-click `index.html` — it opens in your default browser.
3. Bookmark it or drag the file to your bookmark bar for daily access.

Data is saved automatically in `localStorage` and persists between sessions.

> **Tip:** Data is tied to the specific browser you use. Before clearing browser data or switching browsers, use Export Backup first.

---

## Tracked habits

| Habit | Type | Default Goal |
|-------|------|-------------|
| Gym | Checkbox | Done / not done |
| Meditation | Checkbox | Done / not done |
| No Junk Food | Checkbox | Done / not done |
| Water | Number (+/−) | 8 glasses/day (configurable) |
| Reading | Number (+/−) | 20 min/day (configurable) |
| Running | Number (±0.5 km, typeable) | 3 km/session (configurable) |
| Weight | Decimal input | Optional — no streak, no completion % |

---

## Features

### Today tab
- Check off habits, log numbers, log today's weight.
- Progress bar shows how many of the 6 trackable habits are complete (weight is excluded).
- Streak counter next to each habit (🔥 appears at 3+ consecutive days).
- Weight section shows today vs yesterday, vs 7 days ago, vs first ever logged weight.

### Calendar tab
- Monthly grid: green = all habits done, yellow = partial, gray = nothing logged.
- Tap any past day to open an edit modal (edits habits + weight for that day).

### Stats tab
- Current streak and all-time best streak per habit.
- Water totals: this week / this month.
- Reading totals: this week / this month.
- Running totals: this week / this month / longest run / average session.
- Monthly completion rate.

### Progress tab (charts — requires internet for Chart.js)
- **Weight chart:** line chart of daily weight + 7-day moving average. Time ranges: 7d, 30d, 90d, All time. Optional goal weight shown as a dashed line. Summary: start, current, total change, distance to goal.
- **Running chart:** bar chart of km/day for the last 30 days. Goal line overlay. Bars turn green on days you hit your goal.
- **Habit consistency:** for each checkbox habit, a progress bar showing completion % over the last 30 days, plus best streak.

### Data tab
- **Settings & Goals:** configure goal weight (kg), running goal (km/session), water goal (glasses), reading goal (minutes).
- **Export Backup:** downloads a JSON file with all habits data and settings.
- **Import Backup:** restores from a previously exported JSON file (supports both the old and new export format).
- **View All Data:** readable text breakdown of every logged day.

---

## Backing up and restoring

### Export
Go to **Data → Export Backup**. Downloads `habit-tracker-YYYY-MM-DD.json`.

### Import
Go to **Data → Import Backup**, select a backup file. This overwrites current data — export first if needed.

---

## Opening on your phone

1. Same Wi-Fi as laptop.
2. Find your laptop's IP (e.g. `192.168.1.42`).
3. Serve locally:
   ```
   cd habit-tracker
   python3 -m http.server 8080
   ```
4. On your phone: `http://192.168.1.42:8080`

---

## Adding a new habit

Open `index.html` in a text editor. Find `const HABITS = [` near the top of the `<script>` block.

### Checkbox habit
```js
{ key:'coldShower', name:'Cold Shower', icon:'🚿', type:'bool' },
```

### Integer number habit (like Water or Reading)
```js
{ key:'pushups', name:'Push-ups', icon:'💪', type:'num', unit:'reps', step:1 },
```
Then add a goal to `cfg` defaults and the Settings UI if you want it configurable.

### Decimal step habit (like Running)
```js
{ key:'cycling', name:'Cycling', icon:'🚴', type:'run', unit:'km', step:0.5 },
```

Field reference:
- `key` — unique camelCase identifier (used in storage, no spaces)
- `name` — display name
- `icon` — any single emoji
- `type` — `'bool'` | `'num'` | `'run'`
- `unit` — label shown next to the number
- `step` — increment for +/− buttons

For `type:'run'`, the goal is read from `cfg.runGoal` (set in Settings). If you add a second run-type habit, you'd need to add a separate goal key.

---

## Data format (export JSON)

```json
{
  "habits": {
    "2026-05-07": {
      "gym": true,
      "meditation": false,
      "noJunkFood": true,
      "water": 9,
      "reading": 35,
      "running": 4.5,
      "weight": 78.3
    }
  },
  "settings": {
    "goalWeight": 75,
    "runGoal": 3,
    "waterGoal": 8,
    "readGoal": 20
  }
}
```

Old exports (plain habit object without the `habits`/`settings` wrapper) are still imported correctly.

---

## Migration from previous version

Existing `localStorage` data is migrated automatically on first load — a `running: 0` field is added to any days that don't have it. No manual steps needed.
