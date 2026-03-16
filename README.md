# BAiCi Staff Builder

A single-file, zero-dependency staffing Gantt chart builder built for enterprise project planning. Open one HTML file in any modern browser — no server, no install, no build step required.

![Staff Builder Screenshot](screenshot.png)
<!-- Replace with an actual screenshot when available -->

---

## Quick Start

```bash
# Option 1: Open directly in browser (no server needed)
open ajo_staffing_gantt_builder.html

# Option 2: Serve locally (e.g., for iframe embeds or file:// restrictions)
python3 serve.py
# Then visit http://localhost:8080
```

That's it. No npm, no dependencies, no configuration files.

---

## Overview

Staff Builder lets you plan staffing for a project by defining roles, assigning start/end weeks and FTE percentages, grouping work into phases, and visualizing everything as a clean enterprise-style Gantt chart. It auto-saves all work to `localStorage` so your data persists across browser sessions.

**Key capabilities at a glance:**

| Capability | Details |
|---|---|
| Resource planning | Add/remove/reorder roles with inline editing |
| Gantt visualization | Flat SVG bars, swimlane grouping, phase bands |
| Cost tracking | Optional FTE-based cost estimate with currency choice |
| Milestones | Diamond markers with dashed guide lines |
| Today marker | Amber pill + dashed line, auto-placed from start date |
| Export | Download as SVG vector or 3-sheet Excel workbook |
| Persistence | Auto-saved to `localStorage` on every change |

---

## Features

### Resource Editor Table

The left-hand editor table is the source of truth for all staffing data.

| Column | Description |
|---|---|
| Toggle | Checkbox to mark a role as active or inactive. Inactive roles are grayed out and excluded from stats. |
| Role / Name | Free-text field. Identifies the person or role. |
| Type | Dropdown (Onshore / Offshore + any custom types you create). Drives swimlane grouping and heatmap color. |
| Tag | Short label displayed on the Gantt bar (e.g., `PM`, `FE`, `BE`). |
| Start Week | Week number the role starts (1-based). |
| End Week | Week number the role ends (inclusive). |
| FTE% | Allocation: `100%`, `50%`, or `25%`. Controls bar opacity and capacity calculations. |
| Annotation | Optional free-text note shown alongside the bar when bar labels are set to show annotations. |

All edits render the chart live — no save button needed.

**Reordering:** Use the up/down arrow buttons on each row to move roles within the table. Order is preserved in exports.

---

### Gantt Chart (SVG)

The chart is rendered as an inline SVG and updates on every editor change.

- **Flat enterprise-style bars** — no gloss, no 3D effects. Clean and presentation-ready.
- **FTE opacity** — `100%` allocation renders as a solid bar; `50%` is medium opacity; `25%` is light. Gives an instant visual read on part-time vs. full-time coverage.
- **Phase header bands** — colored bands span the top of the chart for each defined phase, showing phase name and date range (or week range if no start date is set).
- **Swimlane grouping** — roles are grouped by type (e.g., `ONSHORE · 7 roles / OFFSHORE · 8 roles`) with a labeled divider row between groups. Toggle this on/off in Settings.
- **Milestone diamonds (◆)** — milestones appear as diamond markers in a dedicated row at the top of the chart, with dashed vertical guide lines running the full height of the chart.
- **TODAY marker** — when a project start date is set, an amber pill label and dashed vertical line mark the current date on the chart. Toggle on/off in Settings.
- **Capacity heatmap** — a stacked bar heatmap at the bottom of the chart shows total FTE load per week, broken down by type (e.g., onshore vs. offshore blend). Toggle on/off in Settings.
- **Week labels** — display as `W1`, `W2`, etc. by default. When a start date is set, labels switch to formatted calendar dates (e.g., `Jan 6`). Interval is configurable.

---

### Phases Modal

Accessed via the **Phases** button in the toolbar.

- Add or remove project phases
- Set a name, start week, end week, and color per phase
- Colors are shown as header bands on the chart and in the legend
- Changes update the chart and phase legend live

---

### Types Modal

Accessed via the **Types** button in the toolbar.

- Default types: **Onshore** and **Offshore**
- Add custom types (e.g., `Vendor`, `Contractor`, `Embedded`)
- Assign a color per type — used in swimlane headers and the capacity heatmap
- Remove any type that has no assigned resources

---

### Milestones Modal

Accessed via the **Milestones** button in the toolbar.

- Add or remove milestone events
- Set a name, week, and color per milestone
- Milestones render as ◆ diamonds in a dedicated row above the role bars
- Dashed vertical guide lines extend from each diamond through the full chart height

---

### Settings Modal

Accessed via the **Settings** button in the toolbar.

| Setting | Description |
|---|---|
| Total Weeks | Sets the chart width (number of week columns). |
| Start Date | Optional. Enables calendar date labels on week headers and activates the TODAY marker. |
| Week Label Interval | How often week labels appear (e.g., every 1, 2, or 4 weeks). |
| Capacity Chart | Toggle the heatmap at the bottom of the chart on/off. |
| Row Height | Pixel height of each resource row in the chart. |
| Bar Label Style | Choose what to show on bars: `Tag`, `Annotation`, `Both`, or `None`. |
| Show Cost | Toggle cost estimation on/off. When on, Est. Cost appears in the header stats. |
| Currency | Choose display currency: USD, EUR, GBP, AUD, or CAD. |
| Swimlane Grouping | Toggle type-based swimlane grouping on/off. |
| Today Marker | Toggle the amber TODAY marker line on/off. |
| Reset to Defaults | Clears all data and resets to the default state. Cannot be undone. |

---

### Header Stats (Live)

The project header displays four auto-calculated stats that update in real time:

| Stat | Calculation |
|---|---|
| Active Resources | Count of all checked (active) roles |
| Weeks | Total project weeks from settings |
| Peak FTE | Highest single-week FTE sum across all active roles |
| Est. Cost | Sum of (FTE × hourly rate × hours/week × weeks) when cost is enabled |

Click the project title or subtitle in the header to edit them inline.

---

## Export

### Export SVG

The **Export SVG** button downloads the current Gantt chart as a `.svg` vector file. The SVG is self-contained and can be opened in Illustrator, Figma, or any vector editor, or embedded directly in a document or slide.

### Export XLS

The **Export XLS** button generates a SpreadsheetML `.xls` workbook with three sheets:

| Sheet | Contents |
|---|---|
| **Resources** | Full role table — name, type, tag, start week, end week, FTE%, annotation |
| **Gantt** | Visual chart with colored cells per type/FTE. Phase header row at top. Swimlane headers between type groups. |
| **Summary** | Project stats (title, dates, active resources, peak FTE, est. cost), phases table, milestones table, type breakdown |

The workbook opens in Microsoft Excel, LibreOffice Calc, and Google Sheets (via import).

---

## Persistence & Autosave

All project data is automatically saved to `localStorage` on every change — no manual save required.

- Data survives page refreshes and browser restarts
- Settings (total weeks, start date, etc.) are also persisted
- **localStorage is per-browser and per-origin** — data is not synced across devices or browsers
- To reset everything, use **Settings → Reset to Defaults**

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Escape` | Close any open modal |

All other interactions are mouse/click-based. Inline text fields support standard browser editing shortcuts (`Cmd/Ctrl+A`, `Cmd/Ctrl+Z`, etc.).

---

## Tech Stack

| Layer | Details |
|---|---|
| Runtime | Pure vanilla JavaScript — no frameworks, no libraries |
| Rendering | Inline SVG (dynamically generated via JS DOM helpers) |
| Styling | Hand-written CSS with BAiCi brand variables (`--bci-900`, `--bci-blue-500`, etc.) |
| Storage | `localStorage` (browser-native, no backend) |
| Export | Blob URLs for SVG; SpreadsheetML XML for XLS |
| Delivery | Single self-contained `.html` file |

No npm. No build step. No server required.

---

## Limitations

- **No undo/redo** — changes are immediate and irreversible (except Reset to Defaults)
- **localStorage only** — data is not synced across devices or browsers
- **SVG viewBox is fixed at 1080px wide** — scales with the container but does not reflow
- **No multi-user / collaboration** — single-user, single-browser tool
- **No import** — there is no way to import a previously exported XLS back into the tool

---

## Version History

| Version | Description |
|---|---|
| **v1.0** | Initial release — basic resource table and Gantt SVG |
| **v1.1** | Gantt layout and design fixes |
| **v2.0** | Editable title/subtitle, header stats, types modal, phases modal, settings modal, capacity heatmap, SVG export |
| **v3.0** | Flat enterprise chart style, FTE dropdown, expanded default role roster |
| **v4.0** | TODAY marker, swimlane grouping by type, milestone diamonds |
| **v4.1** | Live sync for all inputs, capacity type blend, live phase legend updates |
| **v4.2** | Export XLS — 3-sheet SpreadsheetML workbook |
| **v4.3** | localStorage autosave — work persists across browser sessions |

---

## License

Internal tool. All rights reserved — BAiCi.
