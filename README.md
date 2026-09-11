# CDP Standard Template Library

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
- 44px minimum touch targets on mobile controls.
- Semantic landmarks, real heading order, `aria-label` on icon-only controls, `aria-current` on active nav.

## Responsive

Breakpoints: 1280 (API code column drops), 1180 (article right rail hides), 1100 (header nav collapses to a menu), 980 (support pane unpins), 900 (side nav hides, facets become a sheet), 860 (photo mask off).

## Icons

[Lucide](https://lucide.dev) via CDN, `data-lucide` attributes, initialized in `componentDidMount`.

## Fonts

Inter only, weights 400/500/600. Hierarchy is carried by scale and color, not weight.
