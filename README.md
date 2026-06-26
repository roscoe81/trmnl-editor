# TRMNL Visual Editor

A browser-based WYSIWYG editor for [TRMNL](https://usetrmnl.com) e-ink display templates and previews. No installation required — runs entirely in your browser.

**Live editor:** https://roscoe81.github.io/trmnl-editor/

---

## What it does

TRMNL displays are 800×480 pixels. Editing HTML/Liquid templates by hand is error-prone and hard to visualise. This editor gives you a live 800×480 canvas where you can:

- **Click** any element to select it and inspect its CSS classes and inline styles
- **Double-click** text to edit it in place
- **Drag sections** to reorder them
- **Insert new elements** from the palette (sensor rows, calendar events, forecast columns, footer blocks, stat blocks, badges, dividers, and more)
- **Delete** selected elements
- **Edit CSS class properties** directly in the properties panel
- **Edit inline styles** on the selected element
- Work with both **preview files** (hardcoded sample data) and **Liquid template files** (with `{{variable}}` and `{% %}` tags preserved losslessly)

---

## How to use

### Starting from scratch

Click **＋ New…** in the toolbar and choose a layout starter under **Preview** or **Template**:

| Layout | Description |
|---|---|
| Blank | Header + empty canvas |
| Two Column | Header + main content area + narrow right sidebar |
| Full Width | Header + single full-width content area |
| Stats + Footer | Header + three large stat blocks + footer bar |

All starters come pre-loaded with the full set of standard TRMNL CSS classes so everything in the palette works immediately.

### Opening an existing file

Click **📂 Open…** and select your TRMNL preview or template HTML file, or **drag and drop** the file onto the editor window.

The editor detects from the filename whether it's a preview or a Liquid template (files containing `template` in the filename are treated as Liquid templates).

### Creating a preview from a template

If you only have a Liquid template and want a preview to edit, open the template and click **⎘ Template → Preview**. The editor will:

- Remove all Liquid control tags (`{% ... %}`)
- Replace every `{{ variable }}` with a sample value (and apply any `upcase` / `downcase` / `capitalize` filter)
- Switch into **Preview** mode with a `*_preview.html` filename, ready to edit and **Download** or **Sync to Template…**

Sample values are guessed from each variable's name — for example `*_temp` → `18`, `*_chance` / `*_humidity` → `42`, `*_time` / `sunrise` → `12:00 PM`, and descriptive names like `description` or `tariff_name` become a humanised version of the name. Filters are applied, so `{{ tariff_name | upcase }}` renders as `SHOULDER`.

For exact sample values, edit the `PREVIEW_SAMPLES` map near the top of the script in `index.html`:

```js
const PREVIEW_SAMPLES = {
  current_temp: '18',
  description:  'Partly Cloudy',
  tariff_name:  'Shoulder',
};
```

Any key listed there overrides the name-based guess. Values not listed fall back to the guess, so you only need to add the ones you care about.

### Editing

- **Select** an element by clicking it — its classes and styles appear in the right panel
- **Edit text** by double-clicking — press Enter or Escape to finish
- **Reorder sections** by dragging them up or down
- **Add elements** by clicking items in the left palette, then clicking where to insert them
- **Delete** the selected element with the Delete button in the properties panel
- **Adjust CSS** using the class properties fields in the right panel

### Saving your changes

Click **⬇ Download** to save the edited file back to your computer with the original filename. For Liquid template files, all `{{variable}}` and `{% %}` tags are restored exactly as they were — the file can be pasted directly into the TRMNL Private Plugin markup field without modification.

---

## Syncing preview edits to your template

The recommended workflow is to edit your **preview file** (which shows real data) until the layout is right, then use **⇄ Sync to Template…** to apply those structural changes to your Liquid template automatically.

1. Open or create a preview file and edit it until it looks the way you want
2. Click **⇄ Sync to Template…** in the toolbar
3. Select your template file when prompted
4. The editor matches each top-level section by its CSS class, reorders the template to mirror the preview, and downloads the result

**What gets synced automatically:**
- Section order is updated to match the preview
- Sections deleted in the preview are removed from the template
- Sections added in the preview that don't exist in the template are included with a `<!-- TODO -->` comment so you know where to add Liquid variables
- CSS changes (borders, fonts, spacing) are carried across to the template

---

## Liquid template support

[Liquid](https://shopify.github.io/liquid/) is the templating language TRMNL uses for Private Plugins. It lets your display show real data rather than hardcoded text.

**Output tags** `{{ }}` insert a value:
```
{{ temperature }}                      → 22.5
{{ temperature | round: 1 }}           → 22.5 (rounded)
{{ name | upcase }}                    → LIVING ROOM
{{ updated | date: "%I:%M %p" }}       → 2:31 PM
```

**Control tags** `{% %}` add logic:
```
{% if temp > 30 %}Too hot!{% endif %}
{% for item in sensors %}{{ item.name }}: {{ item.value }}{% endfor %}
```

> **Note:** TRMNL's Liquid engine does not support `{% endif %}` or `{% endfor %}` block-closing tags. Avoid conditional or loop blocks in templates — use unconditional HTML instead, and send all slots as separate variables (e.g. `cal_0_title`, `cal_1_title` rather than a list).

**How your data gets there:** You POST a JSON payload to TRMNL's API for your plugin. Each key in the JSON becomes available directly as a variable in the template. So if you push `{"temperature": 22.5, "condition": "Sunny"}`, then `{{ temperature }}` outputs `22.5`.

When editing a template file in this editor, Liquid output tags are displayed as non-editable chips (e.g. `⟨temperature⟩`) and control tags are shown inline — both are restored faithfully on download.

---

## Available CSS classes (new files)

New files are pre-loaded with these classes ready to use:

| Class | Purpose |
|---|---|
| `.page` | Outer wrapper — full 800×480 canvas with padding |
| `.header` | Top bar with title and subtitle, separated by a bottom border |
| `.header-title` | Large bold display title |
| `.header-sub` | Small subtitle / timestamp |
| `.body` | Flex row containing left and right columns |
| `.left` | Main content column (flex, grows to fill) |
| `.right` | Narrow right sidebar (210px, left border) |
| `.section-label` | Small uppercase section heading |
| `.sensor-section` | Group of sensor rows with a heading |
| `.sensor-heading` | Heading for a sensor group |
| `.sensor-row` | Single sensor reading (label + value, space-between) |
| `.sensor-name` | Sensor label |
| `.sensor-val` | Sensor value (bold) |
| `.forecast` | Horizontal strip of forecast columns |
| `.forecast-col` | Single forecast time slot (bordered) |
| `.cal-section` | Scrollable list of calendar events |
| `.cal-row` | Single calendar event (time + title) |
| `.footer` | Bottom bar separated by a top border |
| `.footer-block` | Single item in the footer bar |
| `.footer-lbl` | Footer item label |
| `.footer-val` | Footer item value (bold) |
| `.footer-sep` | Vertical divider between footer items |
| `.stat-block` | Large centred stat (label above, big number below) |
| `.stat-label` | Label for a stat block |
| `.stat-value` | Large value in a stat block |
| `.badge` | Bordered highlight box |
| `.badge-label` | Label inside a badge |
| `.badge-value` | Value inside a badge |
| `.col-sep` | Vertical divider between columns |

---

## Supported palette elements

| Element | Description |
|---|---|
| Sensor Section | A labelled group of sensor rows |
| Sensor Row | A single sensor reading with label and value |
| Calendar Event | A row in a calendar section |
| Forecast Column | A single time slot in a forecast strip |
| Footer Block | A labelled value in the footer bar |
| Separator | Vertical divider between footer blocks |
| Col Separator | Vertical divider between stat columns |
| Stat Block | A large centred stat with label and value |
| Badge | A bordered highlight box |
| Section Label | A small uppercase section heading |
| Custom Div | A blank div to style freely |

---

## Compatibility

Works in any modern browser (Chrome, Firefox, Safari, Edge). Drag-to-reorder uses [SortableJS](https://github.com/SortableJS/Sortable).
