# Shift Ledger

Shift Ledger is a lightweight, mobile-friendly work journal for recording completed tasks and reviewing activity across daily, weekly, monthly, and yearly views. It is delivered as one self-contained HTML file with no framework, build process, or backend requirement.

## Features

- Record completed work with a title, project, date, and optional note
- Edit, duplicate, or delete existing entries
- Reuse project names through browser-native suggestions
- Review today's work and at-a-glance weekly totals
- Navigate backward through weekly, monthly, and yearly reports
- Visualize weekly activity, monthly calendar intensity, and yearly totals
- See totals, active days, averages, busiest periods, streaks, and project distribution
- Export week, month, and year reports as formatted A4 PDFs
- Switch between light and dark themes
- Preserve entries and theme preferences in browser storage
- Migrate data from the app's earlier `v1` and `v2` storage formats
- Use a responsive layout with mobile safe-area support

## Tech stack

| Layer | Technology |
| --- | --- |
| Interface | Semantic HTML5 |
| Styling | Vanilla CSS, custom properties, responsive layouts |
| Logic | Vanilla JavaScript in an immediately invoked function |
| Persistence | Browser `localStorage` |
| PDF export | [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 via cdnjs |
| Typography | Archivo and IBM Plex Mono via Google Fonts |

## Project structure

```text
shift-ledger/
└── Shift-Ledger.html   # Complete application: markup, styles, logic, and inline icon
```

## Getting started

### Quick start

1. Download `Shift-Ledger.html`.
2. Open it in a modern web browser.
3. Use the **+** button to record a completed piece of work.

The app works directly from the saved HTML file. No installation, terminal command, dependency download, or development server is required.

An internet connection is needed to load the intended fonts and the jsPDF library. Existing entries and reports remain available in the interface without a backend, but PDF export requires jsPDF to load successfully.

### Optional local server

Serving the file over HTTP is useful while editing it:

```bash
python -m http.server 8000
```

Then visit [http://localhost:8000/Shift-Ledger.html](http://localhost:8000/Shift-Ledger.html).

## Scripts and build process

There is no `package.json`, package-manager configuration, build command, or test script. All application logic is embedded near the end of `Shift-Ledger.html`.

The inline script provides:

- State loading, saving, and legacy-data migration
- Example-data seeding on first launch
- Entry creation, editing, duplication, and deletion
- Today, week, month, and year rendering
- Project aggregation and activity statistics
- Date navigation
- Theme switching
- PDF report generation and download

The sole external JavaScript dependency is:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
```

## Using Shift Ledger

### Log work

Press the floating **+** button, enter what you completed, choose or type a project, select a date, and optionally add a note. Dates cannot be set in the future.

Each entry's options menu allows you to:

- Edit its details
- Record the same work again today
- Delete the entry

### Review activity

Use the bottom navigation to switch between:

- **Today** — today's entries plus current-week totals and project count
- **Week** — daily activity bars, statistics, project allocation, and grouped entries
- **Month** — a calendar heatmap, active-day statistics, busiest day, and project allocation
- **Year** — monthly activity, yearly statistics, busiest month, and project allocation

The week, month, and year views can move into earlier periods. Navigation into a future period is disabled.

### Export reports

The reporting views offer downloadable PDFs:

- Weekly reports include summary metrics, daily activity, project allocation, and entries.
- Monthly reports include summary metrics, weekly activity, project allocation, and entries.
- Yearly reports include summary metrics, monthly activity, and project allocation; individual entries are intentionally omitted.

When embedded in a compatible host that exposes `window.claude.use("downloads")`, the app uses that download service. Otherwise, it uses the browser's standard file-download mechanism.

## Data storage

Shift Ledger stores its state in `localStorage` under:

```text
shiftledger.v3
```

The state contains the entry list, selected theme, and first-run seed status. A typical entry resembles:

```json
{
  "id": "mabc123xy",
  "date": "2026-10-06",
  "title": "Finished the weekly report",
  "project": "Operations",
  "note": "Sent for review",
  "created": 1791288000000
}
```

On startup, the app can migrate entries saved under `shiftledger.v2` or `shiftledger.v1` into the current format.

Data belongs to the browser profile and origin where the file is used. It is not synced across devices, and clearing site data removes it.

## Deployment

Deploy `Shift-Ledger.html` to any static web host or ordinary web server. Because the app is a single file, no server-side routes, environment variables, or build output are needed.

For fully offline deployment, download and host jsPDF and the font files locally, then update their URLs in the HTML. The current file does not include a web app manifest or service worker.

## Customization

Edit `Shift-Ledger.html` directly:

- CSS custom properties control colors, spacing, and theme appearance.
- `seed()` controls the first-launch example entries.
- The `state` object defines persisted application state.
- `KEY` and `OLDS` define the current and legacy storage keys.
- The `makeWeekPdf()`, `makeMonthPdf()`, and `makeYearPdf()` functions configure report contents and filenames.
- The inline Apple touch icon can be replaced with another SVG data URL or a file path.

If the persisted data structure changes, update the storage key and add migration logic so existing users keep their records.

## Current limitations

- Data is local to one browser profile and origin; there is no account or cloud synchronization.
- There is no built-in backup, import, or spreadsheet export.
- Fonts and PDF generation depend on third-party CDNs.
- The page includes mobile-app metadata but no manifest or service worker, so it is not an offline-capable PWA.
- There is no automated test suite or formal build pipeline.

## License

No license is declared in the source file. Add a license before redistribution or outside contributions.
