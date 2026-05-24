# Apps / Games Split — Design

**Date:** 2026-05-24
**Scope:** `www/index.html`, `www/css/style.css`
**Status:** Approved

## Goal

Split the current flat "My Apps" list on pykaso.net into two grouped sections:

- **My Apps** — Skanae, Photo Album Widget, Photo Widget Studio, Roamee
- **My Games** — NotesNinja

The page must remain a single static HTML file with no JavaScript and no build step. Visual style (glass cards, gradient background, small uppercase section labels) stays unchanged.

## Non-goals

- No filter pills, tabs, or interactive switching.
- No per-card badges or tags.
- No side-by-side desktop columns.
- No separate `/apps` and `/games` pages, no routing, no JS.
- No data/JSON extraction or templating; content stays inline in HTML.

These were considered and rejected: the 4-vs-1 split is too small to justify them, and any of them would push the site away from its current minimal static shape.

## Structure

Two sibling `<section>` blocks reusing the same card pattern.

```html
<section class="row work-section" aria-labelledby="apps-heading">
  <h2 id="apps-heading" class="section-title">My Apps</h2>
  <div class="cards-grid">
    <a href="https://skan.ae/" class="card" target="_blank" rel="noopener noreferrer">
      <span class="card-name">Skanae</span>
      <span class="card-desc">Private PDF scanner with on-device OCR &amp; AI</span>
    </a>
    <a href="https://photoalbumwidget.app/" class="card" target="_blank" rel="noopener noreferrer">
      <span class="card-name">Photo Album Widget</span>
      <span class="card-desc">Your memories, always on your Home Screen</span>
    </a>
    <a href="https://photowidgetstudio.eu/" class="card" target="_blank" rel="noopener noreferrer">
      <span class="card-name">Photo Widget Studio</span>
      <span class="card-desc">Design stylish photo widgets with ease</span>
    </a>
    <a href="https://roamee.co/" class="card" target="_blank" rel="noopener noreferrer">
      <span class="card-name">Roamee</span>
      <span class="card-desc">Plan, capture and relive every journey</span>
    </a>
  </div>
</section>

<section class="row work-section" aria-labelledby="games-heading">
  <h2 id="games-heading" class="section-title">My Games</h2>
  <div class="cards-grid">
    <a href="https://notesninja.app/" class="card" target="_blank" rel="noopener noreferrer">
      <span class="card-name">NotesNinja</span>
      <span class="card-desc">Learn music notes the fun way</span>
    </a>
  </div>
</section>
```

NotesNinja moves out of Apps into Games. App order in the Apps section is preserved from the current page (Skanae, Photo Album Widget, Photo Widget Studio, Roamee).

## CSS changes

Rename the app-specific class names to neutral ones, since the same pattern now serves both groups. The styles themselves do not change.

| Current             | New             |
|---------------------|-----------------|
| `.apps-section`     | `.work-section` |
| `.apps-grid`        | `.cards-grid`   |
| `.app-card`         | `.card`         |
| `.app-name`         | `.card-name`    |
| `.app-desc`         | `.card-desc`    |

Add one rule to tighten vertical spacing between consecutive sections so the two groups read as one block of work rather than two disconnected regions:

```css
.work-section + .work-section {
  margin-top: 1.5em;
}
```

The existing `.work-section { margin-top: 3em; padding-bottom: 3em; }` continues to give the Apps section its breathing room from the header above. The `padding-bottom: 3em` on the last section gives the page its bottom gutter.

## Metadata

Update `<meta name="description">` and the matching `og:description` to reflect the mix. Current text enumerates all five product names; the new text should mention the games dimension without becoming a list dump.

- `<meta name="description">` — `Lukáš Gergel is a developer behind iOS apps like Skanae, Photo Album Widget, Photo Widget Studio, and Roamee, plus the NotesNinja music game.`
- `<meta property="og:description">` — `Developer behind Skanae, Photo Album Widget, and the NotesNinja music game.`

Page title stays the same.

## Accessibility

- Each section keeps its own `aria-labelledby` pointing at its `<h2>`, so screen-reader users get a clear "My Apps" / "My Games" announcement when navigating by landmark or heading.
- Card structure (anchor wrapping name + description) is unchanged, so existing focus behaviour and `:focus-visible` outline still apply.

## Verification

After implementation, open `www/index.html` in a browser and confirm:

1. Two labelled sections render in order: "MY APPS" with 4 cards, "MY GAMES" with 1 card.
2. Spacing between the two section labels is tighter than the spacing between the avatar and the first label.
3. Hover lift, glass background, and focus outline still work on cards in both sections.
4. View source: the description meta tags reflect the new copy.
5. Tab order walks LinkedIn → GitHub → 4 app cards → NotesNinja card.

## Future considerations (not in scope)

If the Games list grows past ~3 entries, revisit whether side-by-side columns on desktop become worthwhile. Filter pills only become useful if the combined list passes ~10 items.
