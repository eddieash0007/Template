# Folio — rules for AI coders

Folio is a framework-free admin template: **one CSS file, one JS file, plain HTML pages.** Editorial look — serif type, hairline rules, outlined controls, a single gold accent. Read this before writing any markup.

## Files
- `css/folio.css` — all tokens and components. Never add another stylesheet; never edit tokens per page.
- `js/folio.js` — all behaviour, wired by `data-fo-*` attributes. Never write inline `onclick` or page `<script>` blocks.
- `components.html` — **the catalog.** Every component, rendered, with copy-paste markup. Copy from it; don't invent.
- `starter.html` — blank page in the full shell. **Every new app page starts as a copy of this file.**
- Example pages: `index.html` (dashboard), `tables.html`, `forms.html`, `profile.html`, `settings.html`, `kanban.html`, `calendar.html`, `inbox.html`, `invoice.html`, `states.html`, `login.html`, `register.html`, `forgot.html`, `404.html`, `500.html`.

## Hard rules
1. **Prefix:** every class is `fo-*`. Don't create unprefixed classes. If you truly need a new component, add it to `folio.css` under a new numbered section *and* add a demo to `components.html`.
2. **Colors, fonts, spacing come from tokens.** Use `var(--color-*)`, `var(--space-1…12)`, `var(--radius-*)`, `var(--shadow-*)`. No hex values, no px spacing literals in markup, no new fonts.
3. **Color is stroke, not fill.** Buttons are outlined. Cards are bordered and transparent. Never fill a button or card with the accent. Tints (`--color-accent-tint`) are allowed for selected/hover states only.
4. **One `.fo-btn-primary` per view region** (page header, modal footer, card footer).
5. **No bold.** Headings use the heading font at 500–600. Emphasis = italic or `font-weight:500`.
6. **Numbers in tables, stats and charts are tabular** — use `.fo-num` or rely on `.fo-table`/`.fo-stat-value`, which already set it.
7. **Icons are Lucide**: `<i data-lucide="name"></i>`. Icon-only buttons need `aria-label`. After injecting HTML containing icons at runtime, call `lucide.createIcons()` (or `Folio.init(el)`).
8. **No emoji, no gradients, no heavy shadows, no sans-serif.**
9. **Dark mode is automatic** — tokens swap under `[data-theme="dark"]`. Never hard-code a color that would break it.
10. Keep the `<head>` from `starter.html` exactly (the inline pre-paint script prevents a theme flash).

## Page anatomy
```html
<body>
<div class="fo-app">
  <aside class="fo-sidebar">…brand + .fo-nav (copy from starter.html)…</aside>
  <div class="fo-scrim"></div>
  <div class="fo-main">
    <header class="fo-topbar">…</header>
    <main class="fo-content">
      <div class="fo-page-header">
        <div><div class="fo-page-kicker">Section</div><h1>Title</h1><p class="fo-page-sub">One line.</p></div>
        <div class="fo-page-actions"><button class="fo-btn fo-btn-primary">Action</button></div>
      </div>
      <!-- content: .fo-grid with .fo-col-N children, .fo-stack, .fo-card … -->
    </main>
    <footer class="fo-footer">…</footer>
  </div>
</div>
<!-- modals/drawers go here, after .fo-app -->
</body>
```
- **Navigation lives in each page's `.fo-sidebar`.** When you add a page, add its `<a class="fo-nav-link">` to the sidebar of *every* page (the same block is in all of them). `aria-current="page"` is set automatically from the URL.
- The same sidebar markup becomes a top bar when `data-layout="top"` — don't build a second nav.

## Layout
- `.fo-grid` = 12 columns; children take `.fo-col-3/4/5/6/7/8/9/12`. They collapse responsively.
- `.fo-auto-grid` for card galleries. `.fo-stack` / `.fo-stack-lg` for vertical rhythm. `.fo-row` / `.fo-between` for horizontal groups.
- Use `gap`, never margins between siblings.

## Behaviour attributes (folio.js)
| Attribute | Effect |
|---|---|
| `data-fo-toggle="theme \| layout \| sidebar"` | Toggle dark mode, side/top nav, collapse sidebar (persisted) |
| `data-fo-set-theme` / `data-fo-set-layout` on radio inputs | Set preference explicitly (`value="light|dark"`, `"side|top"`) |
| `.fo-dropdown > [data-fo-dropdown] + .fo-menu \| .fo-popover` | Dropdown / popover |
| `data-fo-open="#id"` · `data-fo-close` | Open/close `.fo-overlay#id` (modal or drawer) |
| `[data-fo-tabs] > .fo-tab[aria-controls="panelId"]` | Tabs |
| `data-fo-toast="msg"` (+ `-title`, `-type="success|danger|info"`) | Toast on click |
| `data-fo-dismiss` | Remove closest `.fo-alert`, `.fo-banner`, `.fo-tag` |
| `data-fo-copy` | Copy nearest `.fo-code` (or the attribute's value) |
| `data-fo-tooltip="text"` (+ `data-fo-tooltip-pos="bottom|right"`) | CSS tooltip |
| `form[data-fo-validate]` (+ `data-success`, `data-success-title`, `data-redirect`) | Validates `required`, `type=email`, `minlength`, `pattern`, `data-match="#id"`; writes into `.fo-error` |
| `data-fo-count="#out"` on textarea | Live character count |
| `data-fo-output="#out"` on range | Live value |
| `table[data-fo-table]` + `th[data-sort="text|num"]` | Sorting. Optional: `data-search`, `data-filter` (selects with `data-key` matching row `data-*`), `data-page-size`, `data-pagination`, `data-info`, `data-bulkbar`; `[data-fo-select-all]` checkbox |
| `.fo-chart[data-type="line|area|bar|donut|spark"]` | SVG chart. `data-series` (JSON array or array of arrays), `data-labels`, `data-names`, `data-prefix`, `data-suffix`, `data-center`, `data-center-label`. Give it a height. |
| `[data-fo-kanban]` | Drag-and-drop between `.fo-kanban-list` |
| `[data-fo-calendar]` + `data-events='[{"date":"YYYY-MM-DD","title":"…","time":"09:30","kind":"a|b|c"}]'` | Month calendar |
| `[data-fo-select-list] > [data-target="#panel"]` | Master/detail selection (inbox) |
| `data-fo-print` | `window.print()` |
| `data-fo-search` on an input | Focused by ⌘K or `/` |

JS API: `Folio.toast({title, message, type})`, `Folio.openModal('#id')`, `Folio.closeModal()`, `Folio.renderCharts()`, `Folio.init(rootEl)` (wire up dynamically inserted markup), `Folio.setTheme('dark')`, `Folio.setLayout('top')`.

## Writing copy
Sentence case. Short, specific, calm. Buttons are verbs ("Create report", not "Submit"). Empty states: one mark, one heading, one sentence, one action.

## Checklist before you finish a page
- [ ] Started from `starter.html`; nav link added to all sidebars
- [ ] Only `fo-*` classes and tokens; no hex, no inline fonts
- [ ] Exactly one primary button per region
- [ ] Every list/table has an empty state
- [ ] Icon-only buttons have `aria-label`; inputs have `<label for>`
- [ ] Checked in dark mode and at 380px wide
