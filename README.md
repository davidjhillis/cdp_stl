# CDP Standard Template Library

**Live templates: https://davidjhillis.github.io/cdp_stl/**

Standard content-delivery-platform templates for Ingeniux documentation portals.
One self-contained file: open `index.html` in a browser — no build step, no package manager.

## Screens

Use the puck at the bottom right to switch screens.

| Screen | Notes |
| --- | --- |
| Home | Photo-band hero, recommended cards, solution deck, directory grid |
| Group landing | Photo band + product grid (grouping term is configurable) |
| Reader / Article | 12-col doc grid, sticky action rail, collapsible TOC, right widgets, feedback dialog |
| Search results | Facet rail on desktop, filter sheet on mobile, AI answer, active-filter chips |
| What's New | Pinned release + filtered card feed |
| API index | Endpoint table, use cases, auth, rate limits |
| API endpoint | Three columns: nav, reference, sticky code column |
| API playground | Modal with endpoint picker, request builder, live response |
| API keys | Key table + create-key modal with one-time reveal |
| Changelog | Version-anchored release notes |
| Support | Case form with live article deflection in the right pane |
| User profile | Overview, saved, subscriptions, downloads, settings |
| Sign in | Split layout, SSO + credentials |
| 404 | Search-first recovery |
| Component catalog | Page templates + live component specimens (the ICE/page-builder spec) |

## Theming

All visual decisions are CSS custom properties in the `<style>` block at the top of `index.html`.

- **Color** — one accent hex drives the family: `--accent`, `--accent-2`, `--accent-soft`, `--deep`, plus `--deep-rgb` / `--accent-rgb` for alpha stops. The hero wash and solution-deck ramps derive from these, so a rebrand is a one-value change.
- **Neutrals** — `--bg`, `--surface`, `--field`, `--line`, `--line-2`, `--ink`, `--ink-2`, `--ink-3`.
- **Geometry** — `--r-card`, `--r-outer`, `--r-btn`, `--pad-card`, `--hero-curve`.
- **Motion** — `--dur-fast` (feedback), `--dur` (UI), `--dur-slow` (entrances), `--dur-deck`, plus `--ease-out` / `--ease-in` / `--ease-spring`.
- **Elevation** — `--elev-0` rest, `--elev-1` hover, `--elev-2` popover, `--elev-3` overlay. Cards are flat at rest and lift on hover.

Runtime tweaks (accent, density, hero curve, grouping label, motion, elevation) are applied in `applyTheme()` in the logic class.

## Component chassis

- `.hub-card` — the card contract: flat field, 12px radius, 20/30 padding, no resting border or shadow, lift on hover, mark top-right. Variants: `.hub-card--dir` (directory list), `.hub-tile` (compact).
- `.exp-card` / `.exp-deck` — flex-basis accordion with a `:has()` hand-off so one panel is always open.
- `.photo` — blurred plate + masked sharp layer + brand wash; guarantees text contrast over any photograph.
- `.prose`, `.callout`, `.doc-table`, `.code` — article content styles.
- `[data-motion]` — declarative entrances (`fade-up`, `scale-in`, `slide-right`) revealed by an IntersectionObserver. The hidden state is gated on `html.js`, so content is visible and static if scripts fail.

## Accessibility

- Visible focus rings on every interactive element (`:focus-visible`, accent-colored).
- `prefers-reduced-motion` zeroes all durations and hover transforms in a single rule.
- 44px minimum touch targets below 760px on buttons, selects and the screen switcher. Text links inside prose keep their type size (WCAG 2.5.8 exempts inline text links).
- Semantic landmarks, real heading order, `aria-label` on icon-only controls, `aria-current` on active nav.

## Responsive

Verified with no horizontal page overflow and no text collisions on all 14 screens at 320, 375, 414, 768, 1024, 1280 and 1440.

Breakpoints, widest first:

| Width | What changes |
| --- | --- |
| 1280 | API code column drops |
| 1180 | Article right rail hides; API reference splits to one column |
| 1100 | Header nav collapses to a menu |
| 1000 | Solution deck stacks |
| 980 | Support pane unpins |
| 900 | Side nav hides, facets become a sheet, `.doc-grid` collapses to one column, API keys table drops its head row |
| 860 | Photo mask off; pinned release stacks |
| 820 | Playground splits to one column |
| 760 | Masthead compresses: 16px gutters, 44px targets, lockup trims, duplicate profile button hides. Interactive heights raise to a 44px minimum |
| 640 | Article header stacks: breadcrumb collapses to the parent link, action rail wraps to its own row, the bar unpins. Hero padding and h1 come down |
| 480 | Card grids collapse to one column; 404 and results toolbar stop being sized by max-content |
| 360 | Masthead wordmark hides, leaving the mark |

Two notes for anyone extending this. Collapsing a multi-column grid means overriding `grid-template-columns` **and** the `gap`, not just the child spans — twelve `minmax(0,1fr)` tracks with a 32px gap left standing resolve every track to 0 and size the row from the gaps alone. And because layout is carried by inline `style` attributes rather than classes, the small-screen rules need `!important` to win; the `data-*` hooks on the masthead, article header and 404 exist so those overrides have something stable to target.

## Icons

[Lucide](https://lucide.dev) via CDN, `data-lucide` attributes, initialized in `componentDidMount`.

## Fonts

Inter only, weights 400/500/600. Hierarchy is carried by scale and color, not weight.
