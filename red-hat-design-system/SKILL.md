---
name: red-hat-design-system
description: Reference guide for Red Hat Design System Web Components including installation, theming, accessibility, and component usage patterns. Use when building Red Hat branded experiences, working with RHDS components (rh-*), implementing design tokens, or when the user mentions Red Hat Design System, RHDS, @rhds/elements, or Red Hat Web Components.
---

# Red Hat Design System (RHDS)

Reference guide for using Red Hat Design System Web Components to build branded, accessible experiences.

## What is RHDS?

The Red Hat Design System (`@rhds/elements`) is a collection of 46 framework-agnostic Web Components built with Lit. It provides ready-made components with Red Hat branding, accessibility, and design guidelines built in.

**Key features:**
- Framework-agnostic (works with React, Vue, vanilla JS)
- Built-in accessibility (WCAG compliant)
- Powerful theming system (color palettes, design tokens)
- Comprehensive documentation at https://ux.redhat.com

## Installation

### Method 1: Red Hat CDN (Recommended for Red Hat domains)

**Important:** The Red Hat CDN only accepts requests from `*.redhat.com` domains. For local development, configure `/etc/hosts` to forward `localhost` to a `*.redhat.com` domain.

Add import map to `<head>`:

```html
<script type="importmap">
{
  "imports": {
    "@rhds/elements/": "https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/elements/"
  }
}
</script>
```

Load components:

```html
<rh-button>Click me</rh-button>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
</script>
```

### Method 2: NPM

```bash
npm install @rhds/elements
```

Import and use:

```javascript
import "@rhds/elements/rh-button/rh-button.js";
```

### Method 3: jsDelivr CDN (Development only)

**Warning:** Do not use on production Red Hat domains.

```html
<script type="importmap">
{
  "imports": {
    "@rhds/elements/": "https://cdn.jsdelivr.net/npm/@rhds/elements@latest/elements/"
  }
}
</script>
```

### Design Tokens

Always include design tokens CSS in `<head>`:

```html
<link rel="stylesheet" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/tokens@latest/css/global.css">
```

## Component Usage Patterns

### Vanilla HTML

```html
<rh-card>
  <h2 slot="header">Card Title</h2>
  <p>Card content goes here.</p>
  <rh-cta slot="footer">
    <a href="/learn-more">Learn more</a>
  </rh-cta>
</rh-card>

<script type="module">
  import "@rhds/elements/rh-card/rh-card.js";
  import "@rhds/elements/rh-cta/rh-cta.js";
</script>
```

### React (with @lit-labs/react)

```bash
npm install @lit-labs/react
```

```jsx
import { Button } from "@rhds/elements/react/rh-button/rh-button.js";
import { Card } from "@rhds/elements/react/rh-card/rh-card.js";

function App() {
  return (
    <Card>
      <h2 slot="header">React Card</h2>
      <Button onClick={() => alert('Clicked!')}>
        Click me
      </Button>
    </Card>
  );
}
```

### Vue

```vue
<script>
import "@rhds/elements/rh-card/rh-card.js";
export default {
  name: "MyComponent"
};
</script>

<template>
  <rh-card>
    <h2 slot="header">Vue Card</h2>
    <p>Content here</p>
  </rh-card>
</template>
```

## Theming

### Color Palettes

RHDS provides six color palettes that can be applied to container elements:

| Palette | Use Case |
|---------|----------|
| `lightest` | Lightest backgrounds |
| `lighter` | Lighter backgrounds |
| `light` | Light backgrounds (default light theme) |
| `dark` | Dark backgrounds (default dark theme) |
| `darker` | Darker backgrounds |
| `darkest` | Darkest backgrounds |

```html
<rh-card color-palette="lightest">
  <h2 slot="header">Light Card</h2>
</rh-card>

<rh-card color-palette="darkest">
  <h2 slot="header">Dark Card</h2>
</rh-card>
```

### Color Schemes

Components automatically adapt to `light` or `dark` color schemes based on their container's palette or the user's system preference.

### Design Tokens

Customize using CSS variables:

```css
:root {
  --rh-color-brand: #ee0000;  /* Brand red */
  --rh-space-lg: 24px;         /* Large spacing */
  --rh-font-size-heading-md: 1.5rem;
}
```

**Token categories:**
- Color (`--rh-color-*`)
- Space (`--rh-space-*`)
- Typography (`--rh-font-*`)
- Border (`--rh-border-*`)
- Box shadow (`--rh-box-shadow-*`)
- Breakpoints (`--rh-breakpoint-*`)

See full token reference: https://red-hat-design-tokens.netlify.app

## Accessibility

### Core Principles

1. **Keyboard Navigation:** All interactive components support keyboard navigation (Tab, Enter, Space, Arrow keys)
2. **Focus Management:** Visible focus indicators on all interactive elements
3. **Screen Reader Support:** Proper ARIA labels and semantic HTML
4. **Touch Targets:** Minimum 44×44px touch targets
5. **Color Contrast:** WCAG AA/AAA compliant contrast ratios

### Common Keyboard Interactions

| Key | Action |
|-----|--------|
| `Tab` | Move to next interactive element |
| `Shift+Tab` | Move to previous interactive element |
| `Enter` / `Space` | Activate button or link |
| `Escape` | Close dialog or popover |
| `Arrow keys` | Navigate within accordions, tabs, menus |

### Best Practices

- Provide descriptive `aria-label` for icon-only buttons
- Use semantic HTML (headings, lists, landmarks)
- Ensure focus order follows logical reading order
- Test with keyboard only (no mouse)
- Test with screen readers (NVDA, JAWS, VoiceOver)

## Common Components Quick Reference

### Actions & Interactions

**rh-button** - Clickable buttons for triggering actions
- Variants: `primary`, `secondary`, `tertiary`, `play`, `close`
- Props: `variant`, `danger`, `disabled`, `icon`
- Use primary sparingly (one per page)

**rh-cta** - Call-to-action links for navigation
- Use for navigation, not actions
- Props: `variant` (`primary`, `secondary`, `default`)

**rh-switch** - Toggle between two states
- Props: `checked`, `disabled`

### Layout & Containers

**rh-card** - Content container with header/footer slots
- Slots: `header`, `footer`
- Props: `color-palette`
- See [card patterns](/patterns/card/) for variants

**rh-tile** - Clickable card with single action
- Must contain exactly one link (in `headline` slot)
- Entire tile is interactive

**rh-surface** - Container that applies theme to children
- Props: `color-palette`

### Navigation

**rh-tabs** - Organize content into tabs
- Child elements: `rh-tab`, `rh-tab-panel`
- Automatic keyboard navigation

**rh-breadcrumb** - Show navigation hierarchy
- Use `<a>` links for each breadcrumb item

**rh-pagination** - Navigate through pages
- Props: `page`, `total`, `per-page`

### Feedback & Status

**rh-alert** - Display important messages
- Props: `state` (`default`, `info`, `success`, `warning`, `danger`)
- Slots: `header`

**rh-badge** - Show counts or status
- Props: `number`, `threshold`

**rh-tooltip** - Contextual help on hover/focus
- Props: `position`
- Attach to any interactive element

**rh-spinner** - Loading indicator
- Props: `size` (`sm`, `md`, `lg`, `xl`)

### Content Display

**rh-accordion** - Expandable/collapsible panels
- Props: `expanded-index` (comma-separated)
- Child elements: `rh-accordion-header`, `rh-accordion-panel`

**rh-dialog** - Modal dialogs
- Methods: `show()`, `showModal()`, `close()`
- Slots: `header`, `footer`

**rh-table** - Data tables
- Use semantic `<table>` markup inside component
- Requires lightdom CSS

**rh-code-block** - Syntax-highlighted code
- Props: `highlight`

### Media

**rh-icon** - Icons from @rhds/icons
- Props: `icon`, `set`
- Sets: `ui`, `microns`, `standard`

**rh-avatar** - User avatars
- Props: `name`, `src`, `size`

**rh-video-embed** - Embedded videos
- Props: `src` (YouTube/Vimeo URLs)

## Component Selection Guidelines

### Button vs. CTA
- **Button:** Use for actions (submit, delete, cancel)
- **CTA:** Use for navigation (links to other pages)

### Card vs. Tile
- **Card:** May contain multiple actions or no actions
- **Tile:** Must contain exactly one action (entire tile is clickable)

### Alert vs. Toast
- **Alert:** Static, persistent messages in page flow
- **Toast:** (Not yet available) Temporary notifications

## Best Practices

### Layout
- Use minimum 4-column width for cards in 12-column grid
- Maximum 3 cards per row
- Left-align buttons in forms and dialogs
- Use `--rh-space-lg` (16px) between grouped buttons
- Use `--rh-space-md` (12px) between stacked buttons

### Content
- **Button labels:** Max 30 characters, 3 words
- **Card titles:** Max 20 characters
- **Card headlines:** Max 50 characters
- **Card body:** Max 165 characters
- Use clear, action-focused language
- Avoid punctuation in button labels

### Hierarchy
- One primary button per page/dialog
- Order buttons left-to-right by importance
- Use danger variant only for destructive actions

### Performance
- Import only components you use
- Modules in `<head>` are deferred automatically
- Multiple imports to same module are deduplicated
- Consider using import maps for better caching

## Lightdom CSS

Some components require "lightdom CSS" for styling slotted content:

```html
<link rel="stylesheet" 
      href="https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/rh-table/rh-table-lightdom.css">
```

Components requiring lightdom CSS:
- rh-table
- rh-footer
- rh-tile (optional)

## Additional Resources

- **All Components:** See [components-reference.md](components-reference.md) for complete catalog of 46 components
- **Code Examples:** See [examples.md](examples.md) for detailed patterns and implementations
- **Official Docs:** https://ux.redhat.com
- **Design Tokens:** https://red-hat-design-tokens.netlify.app
- **GitHub:** https://github.com/RedHat-UX/red-hat-design-system
- **NPM Package:** https://www.npmjs.com/package/@rhds/elements
