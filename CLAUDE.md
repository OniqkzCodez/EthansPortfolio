# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm start          # Serves the site at http://localhost:3333
PORT=8080 npm start  # Override the default port
```

There is no build step, no bundler, and no test suite. Changes to HTML/CSS/JS are reflected immediately on the next page refresh.

## Architecture

This is a single-page static portfolio site for a game developer. The Express server in [server.js](server.js) exists only to serve the files — `app.use(express.static(...))` handles everything; the explicit `/` route is redundant.

The entire UI lives in one file, [index.html](index.html), structured as four anchor-linked sections: **Hero → About → Games → Contact**. There is no JavaScript framework or component system.

### CSS design tokens

All colors, radius, and shadow values are CSS custom properties defined at the top of [css/styles.css](css/styles.css) in `:root`. Modify those variables first before touching individual rules. Responsive breakpoints are at `768px` (collapses two-column layout, hides profile image) and `480px` (hides nav links, reduces section padding).

### Game cards

Each game is an `<article class="game-card" data-type="solo|team">` in [index.html](index.html). To add a game, copy an existing card block and update the `data-type`, tags, title, description, thumbnail, and links. The card thumbnail is either a `<img>` inside `.card-thumb` or the `.placeholder-thumb` div. Thumbnail images live in [images/](images/).

The filter row currently has only **All** and **Team** buttons — there is no Solo button in the HTML yet, though `data-type="solo"` and `.tag-solo` CSS are ready for use.

### JavaScript

[js/main.js](js/main.js) has exactly two responsibilities:
1. **Filter buttons** — toggling `.hidden` on `.game-card` elements based on `data-type`
2. **Scroll fade-in** — dynamically adds `.fade-in` to `.game-card`, `.about-text`, and `.contact-inner`, then uses an `IntersectionObserver` to add `.visible` when each enters the viewport

### Remaining placeholder

The profile photo in the About section (`about-img-placeholder` div) still needs a real image.
