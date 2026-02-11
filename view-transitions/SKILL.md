---
name: view-transitions
description: Implement smooth, animated page transitions using the View Transitions API with Web Components, Shadow DOM, and Lit. Use when adding page transitions, view animations, slide effects, or when working with SPAs that need visual continuity between states.
---

# View Transitions API

Implement smooth, animated transitions between DOM states in Single Page Applications using the View Transitions API with Lit and Web Components.

## When to Use

- Adding page or view transitions in SPAs
- Creating pagination-style slide animations
- Handling state changes with visual continuity
- Implementing transitions across Shadow DOM boundaries
- Building accessible animated transitions

## Browser Support

**ALWAYS** feature-detect before using:

```typescript
if ('startViewTransition' in document) {
  // Use View Transitions API
} else {
  // Fallback: instant update
}
```

**Support:** Chrome/Edge 111+, Safari Tech Preview. Firefox: no support (requires fallback).

## Core Pattern with Lit

```typescript
@customElement('my-view')
export class MyView extends LitElement {
  @state() private _view = 'dashboard';
  @state() private _isTransitioning = false;

  async switchView(newView: string) {
    if (this._isTransitioning) return; // Prevent concurrent transitions

    if ('startViewTransition' in document) {
      this._isTransitioning = true;
      try {
        const transition = (document as any).startViewTransition(async () => {
          this._view = newView;
          await this.updateComplete; // CRITICAL: Wait for Lit to render
        });
        await transition.finished;
      } finally {
        this._isTransitioning = false;
      }
    } else {
      this._view = newView; // Fallback
    }
  }
}
```

**Critical Requirements:**
- Always `await this.updateComplete` in transition callback
- Use `_isTransitioning` flag to prevent concurrent transitions
- Include try-finally for cleanup
- Provide instant fallback for unsupported browsers

## Shadow DOM Integration

**CRITICAL LIMITATION:** `view-transition-name` does NOT work across Shadow DOM boundaries.

### Solution A: Root-Level Transitions (Cross-Shadow)

Capture entire viewport without named transitions:

```css
/* view-transitions.css - Global scope */
::view-transition-old(root),
::view-transition-new(root) {
  animation-duration: 400ms;
  animation-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}
```

Use for: Full-page transitions, cross-component changes, large section transitions.

### Solution B: Named Transitions (Within Shadow)

Use ONLY for elements within the same shadow root:

```css
/* Inside component Shadow DOM */
:host {
  view-transition-name: my-element;
}

::view-transition-old(my-element),
::view-transition-new(my-element) {
  animation-duration: 300ms;
}
```

Use for: Individual component animations, elements that don't cross shadow boundaries.

## Pagination-Style Slide Transitions

Create directional slide animations (forward/backward):

```typescript
async navigateTo(page: number) {
  if (this._isTransitioning) return;

  const direction = page > this._currentPage ? 'forward' : 'backward';
  
  if ('startViewTransition' in document) {
    this._isTransitioning = true;
    document.documentElement.setAttribute('data-transition-direction', direction);
    
    try {
      const transition = (document as any).startViewTransition(async () => {
        this._currentPage = page;
        await this.updateComplete;
      });
      await transition.finished;
    } finally {
      this._isTransitioning = false;
      setTimeout(() => {
        document.documentElement.removeAttribute('data-transition-direction');
      }, 50);
    }
  } else {
    this._currentPage = page;
  }
}
```

**CSS (global scope):**

```css
/* view-transitions.css */
html {
  view-transition-name: none;
}

::view-transition-group(root) {
  animation-duration: 400ms;
  animation-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  overflow: clip;
}

::view-transition-old(root),
::view-transition-new(root) {
  animation-name: none;
  animation-duration: 400ms;
  animation-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  mix-blend-mode: normal;
  opacity: 1 !important;
}

/* Forward: Old slides left, new slides in from right */
html[data-transition-direction="forward"] ::view-transition-old(root) {
  animation-name: slide-out-left !important;
}

html[data-transition-direction="forward"] ::view-transition-new(root) {
  animation-name: slide-in-right !important;
}

/* Backward: Old slides right, new slides in from left */
html[data-transition-direction="backward"] ::view-transition-old(root) {
  animation-name: slide-out-right !important;
}

html[data-transition-direction="backward"] ::view-transition-new(root) {
  animation-name: slide-in-left !important;
}

@keyframes slide-out-left {
  from { transform: translateX(0); opacity: 1; }
  to { transform: translateX(-100%); opacity: 1; }
}

@keyframes slide-in-right {
  from { transform: translateX(100%); opacity: 1; }
  to { transform: translateX(0); opacity: 1; }
}

@keyframes slide-out-right {
  from { transform: translateX(0); opacity: 1; }
  to { transform: translateX(100%); opacity: 1; }
}

@keyframes slide-in-left {
  from { transform: translateX(-100%); opacity: 1; }
  to { transform: translateX(0); opacity: 1; }
}

/* REQUIRED: Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(root),
  ::view-transition-new(root) {
    animation: none !important;
    animation-duration: 0.01ms !important;
  }
}
```

Link in HTML: `<link rel="stylesheet" href="/src/view-transitions.css">`

## XState Integration Pattern

Wrap state machine sends to add transitions:

```typescript
connectedCallback() {
  super.connectedCallback();
  
  this.xstate?.value?.onTransition((state) => {
    const actor = state.children?.detailsActor;
    
    if (actor && !(actor as any)._transitionWrapped) {
      const originalSend = actor.send.bind(actor);
      
      actor.send = (event: any) => {
        const snapshot = actor.getSnapshot();
        
        // Intercept DONE event for backward transition
        if (event.type === 'DONE' && 
            snapshot?.matches('details') &&
            'startViewTransition' in document &&
            !this._isTransitioning) {
          
          this._isTransitioning = true;
          document.documentElement.setAttribute('data-transition-direction', 'backward');
          
          (document as any).startViewTransition(async () => {
            originalSend(event);
            await this.updateComplete;
          }).finished.finally(() => {
            this._isTransitioning = false;
            setTimeout(() => {
              document.documentElement.removeAttribute('data-transition-direction');
            }, 50);
          });
        } else {
          originalSend(event);
        }
      };
      
      (actor as any)._transitionWrapped = true;
    }
  });
}
```

Use for: Complex state machines, bidirectional navigation, intercepting specific transitions.

## Focus Management

**REQUIRED:** Always manage focus after transitions for accessibility:

```typescript
await transition.finished;
await this.updateComplete;

const target = this.shadowRoot?.querySelector('[part="back-button"]');
if (target) {
  (target as HTMLElement).focus();
}
```

## Accessibility Checklist

**MANDATORY** for all implementations:

### 1. Reduced Motion Support
```css
@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(*),
  ::view-transition-new(*) {
    animation: none !important;
    animation-duration: 0.01ms !important;
  }
}
```

### 2. ARIA Live Regions
```typescript
render() {
  return html`
    <div role="status" aria-live="polite" class="sr-only">
      ${this._view === 'details' 
        ? 'Details view loaded. Press Escape to return.' 
        : 'Dashboard view loaded.'}
    </div>
    ${this.renderCurrentView()}
  `;
}
```

### 3. Keyboard Navigation
```typescript
@bound
private _handleKeyDown(e: KeyboardEvent) {
  if (e.key === 'Escape') {
    this.navigateBack();
  }
}

connectedCallback() {
  super.connectedCallback();
  this.addEventListener('keydown', this._handleKeyDown);
}
```

### 4. Focus Management
- Auto-focus appropriate elements after transition
- Return focus to origin element when closing views
- Always `await this.updateComplete` before setting focus

### 5. Screen Reader Helper
```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

## Performance Best Practices

### 1. GPU-Accelerated Properties
**DO:** Animate `transform` and `opacity` only

```css
@keyframes slide-in {
  from { transform: translateX(100%); opacity: 1; }
  to { transform: translateX(0); opacity: 1; }
}
```

**DON'T:** Animate layout properties (left, top, width, height)

### 2. Optimal Timing
- Duration: 300-400ms
- Easing: `cubic-bezier(0.4, 0, 0.2, 1)` (Material Design)

### 3. Prevent Concurrent Transitions
```typescript
@state() private _isTransitioning = false;

async transition() {
  if (this._isTransitioning) return; // Guard
  this._isTransitioning = true;
  try {
    // ... transition
  } finally {
    this._isTransitioning = false;
  }
}
```

### 4. Cleanup Timing
```typescript
await transition.finished;
setTimeout(() => {
  document.documentElement.removeAttribute('data-transition-direction');
}, 50);
```

## Common Patterns

### Pattern 1: Modal to Full-Screen
```typescript
render() {
  if (this._state === 'modal') {
    return html`<pf-modal>${this.renderContent()}</pf-modal>`;
  }
  return html`
    <div part="fullscreen" style="position: fixed; inset: 0; z-index: 1000;">
      <button @click=${this.goBack}>Back</button>
      ${this.renderContent()}
    </div>
  `;
}
```

### Pattern 2: Conditional View Hiding
```typescript
static styles = css`
  :host([data-view="explore"]) [part='dashboard'] {
    display: none;
  }
`;

render() {
  if (this._view === 'explore') {
    this.setAttribute('data-view', 'explore');
  } else {
    this.removeAttribute('data-view');
  }
  return html`
    <div part="dashboard">${this.renderDashboard()}</div>
    ${this._view === 'explore' ? this.renderExplore() : ''}
  `;
}
```

## Testing with Playwright

```typescript
test('view transition works', async ({ page }) => {
  await page.goto('/');
  await page.click('[data-testid="details-link"]');
  
  const hasViewTransitions = await page.evaluate(() => 
    'startViewTransition' in document
  );
  
  if (hasViewTransitions) {
    await page.waitForTimeout(500); // Wait for animation
  }
  
  await expect(page.locator('[part="details"]')).toBeVisible();
});

test('fallback without API', async ({ page, context }) => {
  await context.addInitScript(() => {
    delete (document as any).startViewTransition;
  });
  
  await page.goto('/');
  await page.click('[data-testid="details-link"]');
  await expect(page.locator('[part="details"]')).toBeVisible();
});
```

## Troubleshooting

### Transition doesn't animate
**Cause:** Missing `await this.updateComplete`

**Fix:**
```typescript
// ✅ CORRECT
const transition = (document as any).startViewTransition(async () => {
  this._view = 'details';
  await this.updateComplete; // CRITICAL
});
```

### Flashing during transition
**Cause:** Default fade animations conflicting

**Fix:**
```css
::view-transition-old(*),
::view-transition-new(*) {
  animation-name: none; /* Disable defaults first */
}
```

### Focus lost after transition
**Cause:** Not managing focus

**Fix:**
```typescript
await transition.finished;
await this.updateComplete;
const target = this.shadowRoot?.querySelector('[part="target"]');
if (target) (target as HTMLElement).focus();
```

## Resources

- [Chrome View Transitions Docs](https://developer.chrome.com/docs/web-platform/view-transitions/)
- [Pagination Example](https://view-transitions.chrome.dev/pagination/spa/)
- [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

## Summary

Key principles:
1. Always feature-detect and provide fallbacks
2. Always `await this.updateComplete` in callbacks
3. Always prevent concurrent transitions
4. Always implement reduced motion support
5. Always manage focus appropriately
6. Use root-level transitions for cross-shadow changes
7. Use named transitions only within shadow boundaries
8. Keep animations GPU-accelerated (transform/opacity)
9. Clean up data attributes after transitions
