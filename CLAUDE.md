# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # dev server at localhost:4321
npm run build    # production build → dist/
npm run preview  # preview the production build
```

## Architecture

Astro 4.x static site. Three routes, no external images — all visuals are pure CSS or inline SVG.

| Route | Page file | Layout |
|---|---|---|
| `/` | `src/pages/index.astro` | `src/layouts/Layout.astro` |
| `/demo/1` | `src/pages/demo/1.astro` | `src/layouts/Demo1Layout.astro` |
| `/demo/2` | `src/pages/demo/2.astro` | `src/layouts/Demo2Layout.astro` |

**Portfolio (`/`)** — assembles components from `src/components/` in order: Nav → Hero → Build → Demos → Experience → Specialization → Skills → Contact → Footer.

**Nebula (`/demo/1`)** — AI SaaS landing page. Components in `src/components/demo1/`. Violet/pink/cyan token palette. `ScrollReveal.astro` wires the `.reveal` / `.is-visible` IntersectionObserver.

**Aurelia (`/demo/2`)** — botanical skincare e-commerce. Components in `src/components/demo2/`. Cream/green/gold palette; Fraunces (serif) + Nunito Sans fonts. Scroll reveal, cart counter, and parallax are `<script is:inline>` blocks in `src/pages/demo/2.astro`.

### CSS token isolation

Each demo layout declares its full `:root` token set with `<style is:global>` so tokens never bleed between routes. Never add global demo tokens to the main layout.

### Scroll reveal

- **Portfolio**: `[data-reveal]` attribute → `.is-visible` class, wired in `src/layouts/Layout.astro`.
- **Demos**: `.reveal` class → `.is-visible` class.

Both use IntersectionObserver with `unobserve` after first intersection. Stagger via `transition-delay` is cleared on `transitionend` to prevent hover lag.

## Key CSS pitfalls

**CSS Grid shrinking** — `repeat(N, 1fr)` expands to `minmax(auto, 1fr)`, which won't shrink below content minimum. Use `repeat(N, minmax(0, 1fr))` whenever grid children contain `white-space: nowrap` text, charts, or fixed-width content. Also add `min-width: 0` to grid children.

**SVG text overflow** — SVG `<text>` never wraps. Always add a `<clipPath>` around label text groups as defensive containment, and split long strings into multiple `<text>` elements. Use unique IDs per instance (e.g. `lbl-${product.id}`) to avoid ID collisions when the same SVG shape is rendered multiple times on one page.

**`max-height` animation + `box-sizing: border-box` padding** — with `border-box`, setting `max-height: 0; overflow: hidden` still renders vertical padding. Zero out `padding-top` and `padding-bottom` in the collapsed state and animate them alongside `max-height` to avoid phantom height.

**Touch hover** — wrap all hover transforms in `@media (hover: hover)` to prevent sticky-hover on touchscreens.

Never ever put "Co-authored with Claude" in the git commit messages
Git commit messages should be very professional, descriptive and standard