# Celadon Color Engine

Celadon Color Engine is a token-driven CSS design system focused on semantic color modeling, theme remapping, layered styling, and reusable UI primitives.

It is designed for teams that want a calm, scalable interface foundation where light and dark themes can evolve without rewriting every component. The system favors semantic intent over raw color usage, so surfaces, typography, borders, feedback states, and accents remain coherent across products and platforms.

## What It Solves

- Replaces direct color usage with semantic tokens such as `--c-bg-page`, `--c-text-title`, and `--c-bg-success`
- Enables day/night theme switching through token remapping rather than component-by-component overrides
- Uses `@layer` to keep reset, base, components, and utilities predictable
- Separates platform adaptation from visual component logic
- Provides reusable UI building blocks for cards, buttons, forms, tables, status badges, titles, and more

## Design Principles

- Semantic first: components should consume intent-based variables, not physical color values
- Stable hierarchy: surfaces, text, borders, and state layers should preserve contrast relationships across themes
- Layered architecture: global order should remain explicit and easy to reason about
- Platform-aware, not platform-fragmented: desktop and mobile variants should remap tokens instead of redefining the system
- Calm visual language: the system is built for clarity, restraint, and long-session readability

## Core Capabilities

- `tokens/`: colors, typography, spacing, elevation, z-index, and foundational variables
- `themes/`: theme remaps, including `theme-day` and `theme-night`
- `layers/` and `base/`: reset behavior and shared document-level styling
- `components/`: buttons, cards, forms, tables, timelines, selectors, modal surfaces, titles, and more
- `utilities/`: helpers for layout and interaction support
- `desktop.css`, `web-mobile.css`, `mobile.css`: platform adaptation layers
- `samples/basic/`: a fuller desktop-oriented design system showcase
- `samples/mobile/`: a mobile-first showcase using `web-mobile.css`

## Project Structure

```text
.
├── index.css
├── desktop.css
├── web-mobile.css
├── mobile.css
├── layers/
├── base/
├── tokens/
├── themes/
├── components/
├── utilities/
├── overrides/
├── samples/
└── LICENSE
```

## Architecture

`index.css` is the main entry point. It establishes the global layer order and imports the system in a predictable sequence:

1. `tokens`
2. `themes`
3. `reset`
4. `base`
5. `components`
6. `utilities`
7. `overrides`

The root layer declaration is:

```css
@layer reset, tokens, base, components, utilities;
```

This structure keeps override behavior intentional and makes the system easier to extend as more components and themes are added.

## Theme Model

Celadon uses semantic variables as the public API for components.

Examples:

- `--c-bg-page`
- `--c-bg-card`
- `--c-text-title`
- `--c-text-secondary`
- `--c-bg-success`
- `--brand-rail`
- `--brand-node`
- `--brand-ring`

The day theme is defined by default in `:root`. The night theme remaps shared semantic variables through `.theme-night`, allowing the same component markup to render correctly in both modes.

### Example

```html
<body class="theme-day">
  <div class="styled-card">
    <h2 class="heading-text">Celadon</h2>
    <button class="celadon-btn celadon-btn--primary">Primary</button>
  </div>
</body>
```

Switching to night mode requires only a theme class change:

```html
<body class="theme-night">
  ...
</body>
```

## Quick Start

Import the main entry and then the platform layer that matches your runtime.

### Web Desktop

```css
@import './celadon-color-engine/index.css';
@import './celadon-color-engine/desktop.css';
```

### Web Mobile

```css
@import './celadon-color-engine/index.css';
@import './celadon-color-engine/web-mobile.css';
```

### uni-app Mobile

```css
@import './celadon-color-engine/index.css';
@import './celadon-color-engine/mobile.css';
```

## Platform Adaptation Strategy

Platform files are adaptation layers, not alternate visual systems.

- `desktop.css`: desktop interaction tuning and scrollbar behavior
- `web-mobile.css`: compact spacing and typography remaps for mobile web
- `mobile.css`: `rpx`-based remaps for uni-app consumers

These files should only remap standard tokens and platform capabilities. They should not redefine component visual rules.

## Included UI Primitives

The repository currently includes reusable modules such as:

- `celadon-btn`
- `celadon-card`
- `celadon-status`
- `styled-card`
- `themed-input`
- `themed-border`
- `themed-divider`
- `theme-selector-dropdown`
- `celadon-table`
- `u-celadon-sr-only`
- `u-celadon-hide-scrollbar`

## Showcase Samples

The repository now includes two showcase entry points:

- `samples/basic/`: a desktop-oriented design system overview with spacious layout, semantic token rendering, and broader component coverage
- `samples/mobile/`: a mobile-first showcase focused on compact rhythm, touch-friendly controls, and `web-mobile.css`

Together they demonstrate:

- day/night theme switching
- semantic color token rendering
- brand accent usage
- platform-aware layout adaptation
- buttons, status badges, cards, forms, tables, typography hierarchy, and touch-oriented surfaces

Quick preview entry points:

- `samples/basic/index.html`
- `samples/mobile/index.html`

## Recommended Usage Guidelines

- Build components against semantic tokens, not against raw grays or hex values
- Keep theme switching in token remaps whenever possible
- Place reusable UI modules under `@layer components`
- Place helper classes under `@layer utilities`
- Use accent tokens for emphasis, not for every surface
- Preserve breathing room, contrast rhythm, and visual hierarchy across themes

## Current Status

Celadon Color Engine is currently structured as a pure CSS repository. It is well-suited as:

- a design token foundation
- a reusable UI style package
- a theme system prototype
- a reference implementation for day/night semantic theming

## License

This project is released under the [MIT](./LICENSE) License.
