# Folio

A framework-free admin template. Plain HTML, one stylesheet (`css/folio.css`), one script (`js/folio.js`). Serif type, hairline rules, outlined controls, one gold accent; light and dark themes; sidebar or top-bar navigation.

## Use
Open `index.html` in a browser — no build step. Fonts load from Google Fonts and icons from Lucide (CDN); vendor them locally for offline use.

Start new pages from `starter.html`. Copy components from `components.html`.

## Pages
Dashboard · Customers (table) · Forms · Profile · Settings · Tasks (kanban) · Calendar · Inbox · Invoice · Empty & loading states · Components reference · Blank starter · Sign in · Register · Reset password · 404 · 500

## Customising
All look-and-feel lives in the tokens at the top of `css/folio.css` (`:root` for light, `[data-theme="dark"]` for dark). Change the accent, fonts or spacing there and every page follows.

## Working with AI coders
`CLAUDE.md` holds the rules, the page anatomy and every `data-fo-*` behaviour. Point your assistant at it first.
