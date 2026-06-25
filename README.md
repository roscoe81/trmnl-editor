# TRMNL Visual Editor

A browser-based WYSIWYG editor for [TRMNL](https://usetrmnl.com) e-ink display templates and previews. No installation required — runs entirely in your browser.

**Live editor:** https://roscoe81.github.io/trmnl-editor/

---

## What it does

TRMNL displays are 800×480 pixels. Editing HTML/Liquid templates by hand is error-prone and hard to visualise. This editor gives you a live 800×480 canvas where you can:

- **Click** any element to select it and inspect its CSS classes and inline styles
- **Double-click** text to edit it in place
- **Drag sections** to reorder them
- **Insert new elements** from the palette (sensor rows, calendar events, forecast columns, footer blocks, dividers, and more)
- **Delete** selected elements
- **Edit CSS class properties** directly in the properties panel
- **Edit inline styles** on the selected element
- Work with both **preview files** (hardcoded sample data) and **Liquid template files** (with `{{merge_variables.xxx}}` and `{% %}` tags preserved losslessly)

---

## How to use

### 1. Open the editor
Go to https://roscoe81.github.io/trmnl-editor/ in any modern browser.

### 2. Load your file
Either:
- Click **Open File** and select your TRMNL preview or template HTML file, or
- **Drag and drop** the file onto the editor window

The editor detects from the filename whether it's a preview or a Liquid template and handles each appropriately (files containing `template` in the filename are treated as Liquid templates).

### 3. Edit
- **Select** an element by clicking it — its classes and styles appear in the right panel
- **Edit text** by double-clicking — press Enter or Escape to finish
- **Reorder sections** by dragging them up or down
- **Add elements** by clicking items in the left palette, then clicking where to insert them
- **Delete** the selected element with the Delete button in the properties panel
- **Adjust CSS** using the class properties fields in the right panel

### 4. Save your changes
Click **Download** to save the edited file back to your computer with the original filename. For Liquid template files, all `{{merge_variables.xxx}}` and `{% %}` tags are restored exactly as they were — the file can be pasted directly into the TRMNL Private Plugin markup field without modification.

---

## Liquid template support

When editing a template file, Liquid output tags (`{{merge_variables.xxx}}`) are displayed as non-editable chips (e.g. `⟨temperature⟩`) and control tags (`{% if ... %}`) are shown inline. Both are restored faithfully on download.

---

## Supported element types (palette)

| Element | Description |
|---|---|
| Sensor section | A labelled group of sensor rows |
| Sensor row | A single sensor reading with label and value |
| Calendar event | A row in a calendar section |
| Forecast column | A single day in a weather forecast strip |
| Footer block | A labelled value in the footer bar |
| Footer separator | A vertical divider between footer blocks |
| Stat block | A flex stat display block |
| Section label | A section heading |
| Custom div | A blank div to style freely |

---

## Running locally (optional)

If you prefer to run the editor locally rather than using the hosted version, simply open `index.html` directly in your browser — no server needed.

---

## Compatibility

Works in any modern browser (Chrome, Firefox, Safari, Edge). Drag-to-reorder uses [SortableJS](https://github.com/SortableJS/Sortable).
