# Red Hat Design System - Code Examples

Detailed code examples and patterns for using RHDS components.

## Installation Examples

### Complete HTML Page - Red Hat CDN

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>RHDS Example</title>
  
  <!-- Design Tokens -->
  <link rel="stylesheet" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/tokens@latest/css/global.css">
  
  <!-- Import Map -->
  <script type="importmap">
  {
    "imports": {
      "@rhds/elements/": "https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/elements/"
    }
  }
  </script>
</head>
<body>
  <rh-card>
    <h2 slot="header">Welcome</h2>
    <p>This is a Red Hat Design System component.</p>
    <rh-cta slot="footer">
      <a href="/learn-more">Learn more</a>
    </rh-cta>
  </rh-card>

  <script type="module">
    import "@rhds/elements/rh-card/rh-card.js";
    import "@rhds/elements/rh-cta/rh-cta.js";
  </script>
</body>
</html>
```

### React App Setup

```jsx
// App.jsx
import { useState } from "react";
import { Button } from "@rhds/elements/react/rh-button/rh-button.js";
import { Card } from "@rhds/elements/react/rh-card/rh-card.js";
import { Badge } from "@rhds/elements/react/rh-badge/rh-badge.js";
import { Alert } from "@rhds/elements/react/rh-alert/rh-alert.js";

export function App() {
  const [count, setCount] = useState(0);
  const [showAlert, setShowAlert] = useState(false);

  const handleIncrement = () => {
    setCount(count + 1);
    setShowAlert(true);
    setTimeout(() => setShowAlert(false), 3000);
  };

  return (
    <>
      {showAlert && (
        <Alert state="success" dismissable onClose={() => setShowAlert(false)}>
          <h3 slot="header">Updated!</h3>
          <p>Count has been incremented.</p>
        </Alert>
      )}
      
      <Card>
        <h2 slot="header">
          Counter Example
          <Badge number={count} />
        </h2>
        <p>Current count: {count}</p>
        <Button slot="footer" onClick={handleIncrement}>
          Increment
        </Button>
      </Card>
    </>
  );
}
```

```jsx
// main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { App } from './App';

// Import design tokens CSS
import '@rhds/tokens/css/global.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### Vue App Setup

```vue
<!-- App.vue -->
<template>
  <div>
    <rh-alert v-if="showAlert" state="success" dismissable @close="showAlert = false">
      <h3 slot="header">Updated!</h3>
      <p>Count has been incremented.</p>
    </rh-alert>

    <rh-card>
      <h2 slot="header">
        Counter Example
        <rh-badge :number="count"></rh-badge>
      </h2>
      <p>Current count: {{ count }}</p>
      <rh-button slot="footer" @click="increment">
        Increment
      </rh-button>
    </rh-card>
  </div>
</template>

<script>
import "@rhds/elements/rh-card/rh-card.js";
import "@rhds/elements/rh-button/rh-button.js";
import "@rhds/elements/rh-badge/rh-badge.js";
import "@rhds/elements/rh-alert/rh-alert.js";

export default {
  name: "App",
  data() {
    return {
      count: 0,
      showAlert: false
    };
  },
  methods: {
    increment() {
      this.count++;
      this.showAlert = true;
      setTimeout(() => {
        this.showAlert = false;
      }, 3000);
    }
  }
};
</script>
```

## Common Patterns

### Basic Card with CTA

```html
<rh-card>
  <h2 slot="header">Get Started</h2>
  <p>Learn how to use Red Hat Design System components in your project.</p>
  <rh-cta slot="footer">
    <a href="/docs">Read the docs</a>
  </rh-cta>
</rh-card>

<script type="module">
  import "@rhds/elements/rh-card/rh-card.js";
  import "@rhds/elements/rh-cta/rh-cta.js";
</script>
```

### Button Groups with Hierarchy

```html
<!-- Primary action with secondary cancel -->
<div style="display: flex; gap: var(--rh-space-lg);">
  <rh-button variant="primary">Save changes</rh-button>
  <rh-button variant="secondary">Cancel</rh-button>
</div>

<!-- Danger action with link cancel -->
<div style="display: flex; gap: var(--rh-space-lg);">
  <rh-button danger>Delete account</rh-button>
  <rh-button variant="link">Cancel</rh-button>
</div>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
</script>
```

### Accordion with Multiple Panels

```html
<rh-accordion expanded-index="0">
  <rh-accordion-header>
    <h3>What is Red Hat Design System?</h3>
  </rh-accordion-header>
  <rh-accordion-panel>
    <p>A collection of Web Components for building Red Hat branded experiences.</p>
  </rh-accordion-panel>

  <rh-accordion-header>
    <h3>How do I install it?</h3>
  </rh-accordion-header>
  <rh-accordion-panel>
    <p>You can install via Red Hat CDN, NPM, or jsDelivr.</p>
  </rh-accordion-panel>

  <rh-accordion-header>
    <h3>Is it accessible?</h3>
  </rh-accordion-header>
  <rh-accordion-panel>
    <p>Yes! All components are built with WCAG compliance in mind.</p>
  </rh-accordion-panel>
</rh-accordion>

<script type="module">
  import "@rhds/elements/rh-accordion/rh-accordion.js";
</script>
```

### Alert Variations

```html
<!-- Success alert -->
<rh-alert state="success">
  <h3 slot="header">Success!</h3>
  <p>Your changes have been saved.</p>
</rh-alert>

<!-- Warning alert with dismiss -->
<rh-alert state="warning" dismissable>
  <h3 slot="header">Warning</h3>
  <p>This action cannot be undone.</p>
</rh-alert>

<!-- Danger alert -->
<rh-alert state="danger">
  <h3 slot="header">Error</h3>
  <p>Something went wrong. Please try again.</p>
</rh-alert>

<!-- Info alert -->
<rh-alert state="info">
  <h3 slot="header">Did you know?</h3>
  <p>You can customize components using design tokens.</p>
</rh-alert>

<script type="module">
  import "@rhds/elements/rh-alert/rh-alert.js";
</script>
```

### Navigation with Tabs

```html
<rh-tabs>
  <rh-tab slot="tab" active>Overview</rh-tab>
  <rh-tab-panel>
    <h3>Overview Content</h3>
    <p>This is the overview tab content.</p>
  </rh-tab-panel>

  <rh-tab slot="tab">Features</rh-tab>
  <rh-tab-panel>
    <h3>Features Content</h3>
    <ul>
      <li>Web Components</li>
      <li>Design Tokens</li>
      <li>Accessibility</li>
    </ul>
  </rh-tab-panel>

  <rh-tab slot="tab">Documentation</rh-tab>
  <rh-tab-panel>
    <h3>Documentation</h3>
    <p>Read the full documentation at ux.redhat.com</p>
  </rh-tab-panel>
</rh-tabs>

<script type="module">
  import "@rhds/elements/rh-tabs/rh-tabs.js";
</script>
```

### Form with Buttons

```html
<form id="myForm">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required>
  </div>
  
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
  </div>

  <!-- Button group aligned left -->
  <div style="display: flex; gap: var(--rh-space-lg); margin-top: var(--rh-space-lg);">
    <rh-button type="submit" variant="primary">Submit</rh-button>
    <rh-button type="reset" variant="secondary">Reset</rh-button>
  </div>
</form>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";

  document.getElementById('myForm').addEventListener('submit', (e) => {
    e.preventDefault();
    console.log('Form submitted!');
  });
</script>
```

### Themed Sections

```html
<!-- Light section -->
<rh-surface color-palette="lightest">
  <div style="padding: var(--rh-space-2xl);">
    <h2>Light Theme Section</h2>
    <rh-card>
      <h3 slot="header">Card in Light Theme</h3>
      <p>This card adapts to the light theme.</p>
    </rh-card>
  </div>
</rh-surface>

<!-- Dark section -->
<rh-surface color-palette="darkest">
  <div style="padding: var(--rh-space-2xl);">
    <h2>Dark Theme Section</h2>
    <rh-card>
      <h3 slot="header">Card in Dark Theme</h3>
      <p>This card adapts to the dark theme.</p>
    </rh-card>
  </div>
</rh-surface>

<script type="module">
  import "@rhds/elements/rh-surface/rh-surface.js";
  import "@rhds/elements/rh-card/rh-card.js";
</script>
```

## Advanced Patterns

### Custom Theming with Design Tokens

```html
<style>
  /* Override design tokens globally */
  :root {
    --rh-color-brand: #0066cc;  /* Custom brand color */
    --rh-space-lg: 24px;        /* Custom spacing */
  }

  /* Override for specific element */
  .custom-button {
    --rh-button-background-color: #00aa00;
    --rh-button-text-color: white;
  }
</style>

<rh-button class="custom-button">Custom Styled Button</rh-button>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
</script>
```

### Responsive Layout with Grid

```html
<style>
  .card-grid {
    display: grid;
    gap: var(--rh-space-lg);
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    padding: var(--rh-space-2xl);
  }
</style>

<div class="card-grid">
  <rh-card>
    <h3 slot="header">Card 1</h3>
    <p>Responsive card in grid layout.</p>
  </rh-card>
  
  <rh-card>
    <h3 slot="header">Card 2</h3>
    <p>Cards automatically wrap to new rows.</p>
  </rh-card>
  
  <rh-card>
    <h3 slot="header">Card 3</h3>
    <p>Maintains consistent spacing.</p>
  </rh-card>
</div>

<script type="module">
  import "@rhds/elements/rh-card/rh-card.js";
</script>
```

### Using Slots

```html
<!-- Card with multiple slots -->
<rh-card>
  <!-- Header slot -->
  <div slot="header">
    <h2>Card with Icon</h2>
    <rh-icon icon="check-circle" set="ui"></rh-icon>
  </div>
  
  <!-- Default slot (body) -->
  <p>This card demonstrates using multiple slots.</p>
  <ul>
    <li>Header slot for title and icon</li>
    <li>Default slot for main content</li>
    <li>Footer slot for actions</li>
  </ul>
  
  <!-- Footer slot -->
  <div slot="footer" style="display: flex; gap: var(--rh-space-lg);">
    <rh-cta variant="primary">
      <a href="/learn-more">Primary action</a>
    </rh-cta>
    <rh-cta>
      <a href="/details">Details</a>
    </rh-cta>
  </div>
</rh-card>

<script type="module">
  import "@rhds/elements/rh-card/rh-card.js";
  import "@rhds/elements/rh-cta/rh-cta.js";
  import "@rhds/elements/rh-icon/rh-icon.js";
</script>
```

### Event Handling

```html
<rh-accordion id="myAccordion">
  <rh-accordion-header>
    <h3>Click to expand</h3>
  </rh-accordion-header>
  <rh-accordion-panel>
    <p>Panel content here.</p>
  </rh-accordion-panel>
</rh-accordion>

<div id="status"></div>

<script type="module">
  import "@rhds/elements/rh-accordion/rh-accordion.js";

  const accordion = document.getElementById('myAccordion');
  const status = document.getElementById('status');

  // Listen for expand event
  accordion.addEventListener('expand', (event) => {
    status.textContent = 'Panel expanded!';
    console.log('Expanded panel:', event.panel);
  });

  // Listen for collapse event
  accordion.addEventListener('collapse', (event) => {
    status.textContent = 'Panel collapsed!';
    console.log('Collapsed panel:', event.panel);
  });
</script>
```

### React State Management with RHDS

```jsx
import { useState, useEffect } from "react";
import { Tabs } from "@rhds/elements/react/rh-tabs/rh-tabs.js";
import { Tab } from "@rhds/elements/react/rh-tab/rh-tab.js";
import { TabPanel } from "@rhds/elements/react/rh-tab-panel/rh-tab-panel.js";
import { Card } from "@rhds/elements/react/rh-card/rh-card.js";
import { Button } from "@rhds/elements/react/rh-button/rh-button.js";

function Dashboard() {
  const [activeTab, setActiveTab] = useState(0);
  const [data, setData] = useState({
    overview: { views: 0 },
    analytics: { conversions: 0 },
    settings: { notifications: true }
  });

  const handleTabChange = (event) => {
    setActiveTab(event.detail.activeIndex);
  };

  const incrementViews = () => {
    setData(prev => ({
      ...prev,
      overview: { views: prev.overview.views + 1 }
    }));
  };

  return (
    <Card>
      <h2 slot="header">Dashboard</h2>
      
      <Tabs onChange={handleTabChange}>
        <Tab slot="tab" active={activeTab === 0}>Overview</Tab>
        <TabPanel>
          <h3>Views: {data.overview.views}</h3>
          <Button onClick={incrementViews}>Increment Views</Button>
        </TabPanel>

        <Tab slot="tab" active={activeTab === 1}>Analytics</Tab>
        <TabPanel>
          <h3>Conversions: {data.analytics.conversions}</h3>
        </TabPanel>

        <Tab slot="tab" active={activeTab === 2}>Settings</Tab>
        <TabPanel>
          <h3>Notifications: {data.settings.notifications ? 'On' : 'Off'}</h3>
        </TabPanel>
      </Tabs>
    </Card>
  );
}

export default Dashboard;
```

## Accessibility Examples

### Keyboard Navigation Implementation

```html
<nav aria-label="Main navigation">
  <rh-button id="menuButton" aria-expanded="false" aria-controls="menu">
    Menu
  </rh-button>
  
  <rh-menu id="menu" style="display: none;">
    <a href="/home">Home</a>
    <a href="/about">About</a>
    <a href="/contact">Contact</a>
  </rh-menu>
</nav>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
  import "@rhds/elements/rh-menu/rh-menu.js";

  const button = document.getElementById('menuButton');
  const menu = document.getElementById('menu');

  button.addEventListener('click', () => {
    const isExpanded = button.getAttribute('aria-expanded') === 'true';
    button.setAttribute('aria-expanded', !isExpanded);
    menu.style.display = isExpanded ? 'none' : 'block';
    
    if (!isExpanded) {
      // Focus first menu item
      menu.querySelector('a').focus();
    }
  });

  // Close on Escape
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && button.getAttribute('aria-expanded') === 'true') {
      button.setAttribute('aria-expanded', 'false');
      menu.style.display = 'none';
      button.focus();
    }
  });
</script>
```

### ARIA Labels for Icon Buttons

```html
<!-- Icon-only button with accessible label -->
<rh-button icon="close" label="Close dialog">
  <!-- No visible text, but label provides accessible name -->
</rh-button>

<!-- Icon button with visible text -->
<rh-button icon="plus">
  Add item
</rh-button>

<!-- Play button (built-in accessible label) -->
<rh-button variant="play" label="Play video"></rh-button>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
</script>
```

### Focus Management in Dialog

```html
<rh-button id="openDialog">Open Dialog</rh-button>

<rh-dialog id="myDialog">
  <h2 slot="header">Confirm Action</h2>
  <p>Are you sure you want to proceed?</p>
  <div slot="footer" style="display: flex; gap: var(--rh-space-lg);">
    <rh-button id="confirmBtn" variant="primary">Confirm</rh-button>
    <rh-button id="cancelBtn" variant="secondary">Cancel</rh-button>
  </div>
</rh-dialog>

<script type="module">
  import "@rhds/elements/rh-dialog/rh-dialog.js";
  import "@rhds/elements/rh-button/rh-button.js";

  const openBtn = document.getElementById('openDialog');
  const dialog = document.getElementById('myDialog');
  const confirmBtn = document.getElementById('confirmBtn');
  const cancelBtn = document.getElementById('cancelBtn');

  // Open dialog and focus first action
  openBtn.addEventListener('click', () => {
    dialog.showModal();
    // Focus is automatically managed by dialog
  });

  // Close dialog and return focus
  confirmBtn.addEventListener('click', () => {
    dialog.close();
    // Focus returns to openBtn automatically
    console.log('Confirmed!');
  });

  cancelBtn.addEventListener('click', () => {
    dialog.close();
  });
</script>
```

### Screen Reader Announcements

```html
<!-- Live region for dynamic updates -->
<div role="status" aria-live="polite" aria-atomic="true" id="status" class="visually-hidden">
  <!-- Screen reader announcements go here -->
</div>

<rh-button id="saveBtn">Save Changes</rh-button>

<style>
  .visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    padding: 0;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    border: 0;
  }
</style>

<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";

  const saveBtn = document.getElementById('saveBtn');
  const status = document.getElementById('status');

  saveBtn.addEventListener('click', async () => {
    // Announce start
    status.textContent = 'Saving...';
    
    // Simulate save
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Announce completion
    status.textContent = 'Changes saved successfully.';
    
    // Clear after delay
    setTimeout(() => {
      status.textContent = '';
    }, 3000);
  });
</script>
```

## Performance Tips

### Lazy Loading Components

```html
<!-- Load critical components immediately -->
<script type="module">
  import "@rhds/elements/rh-button/rh-button.js";
  import "@rhds/elements/rh-alert/rh-alert.js";
</script>

<!-- Lazy load non-critical components -->
<script type="module">
  // Load accordion only when user scrolls to it
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(async (entry) => {
      if (entry.isIntersecting) {
        await import("@rhds/elements/rh-accordion/rh-accordion.js");
        observer.unobserve(entry.target);
      }
    });
  });

  document.querySelectorAll('rh-accordion').forEach((accordion) => {
    observer.observe(accordion);
  });
</script>
```

### Preloading Components

```html
<head>
  <!-- Preload critical components -->
  <link rel="modulepreload" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/elements/rh-button/rh-button.js">
  <link rel="modulepreload" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/elements/rh-card/rh-card.js">
</head>
```

## Troubleshooting

### Red Hat CDN Domain Restrictions

If you encounter CORS errors with the Red Hat CDN:

```bash
# Add to /etc/hosts (macOS/Linux)
127.0.0.1 dev.myproject.redhat.com

# Navigate to http://dev.myproject.redhat.com:3000 instead of localhost:3000
```

### Component Not Rendering

1. Check that import is correct
2. Verify import map is set up
3. Check browser console for errors
4. Ensure component is defined: `customElements.get('rh-button')`

```javascript
// Check if component is registered
if (customElements.get('rh-button')) {
  console.log('rh-button is registered');
} else {
  console.log('rh-button is NOT registered');
}
```

### Styling Issues

If components aren't styled correctly:

1. Ensure design tokens CSS is loaded
2. Check for CSS conflicts
3. Verify color-palette is set on containers
4. Check if lightdom CSS is needed

```html
<!-- Load design tokens -->
<link rel="stylesheet" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/tokens@latest/css/global.css">

<!-- Load lightdom CSS if required -->
<link rel="stylesheet" href="https://www.redhatstatic.com/dssf-001/v2/@rhds/elements@latest/rh-table/rh-table-lightdom.css">
```

---

For more examples and patterns, visit:
- **Official Documentation:** https://ux.redhat.com
- **Pattern Library:** https://ux.redhat.com/patterns/
- **GitHub Repository:** https://github.com/RedHat-UX/red-hat-design-system
