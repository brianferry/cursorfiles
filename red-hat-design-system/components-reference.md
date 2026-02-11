# Red Hat Design System - Components Reference

Complete catalog of all 46 RHDS components organized by category.

## Actions & Interactions

### rh-button
**Purpose:** Clickable buttons for triggering actions

**Use cases:**
- Submit forms
- Trigger actions (delete, save, cancel)
- Open dialogs or modals

**Key attributes:**
- `variant` - `primary` | `secondary` | `tertiary` | `play` | `close` (default: `primary`)
- `danger` - Boolean, for destructive actions
- `disabled` - Boolean, disables interaction
- `icon` - Icon name from @rhds/icons
- `type` - `button` | `submit` | `reset`

**Important notes:**
- Use only one primary button per page
- Order buttons by hierarchy (left to right)
- Play and Close variants don't have disabled state
- Prefer micron icons for better button fit

**Docs:** https://ux.redhat.com/elements/button/

---

### rh-cta
**Purpose:** Call-to-action links for navigation

**Use cases:**
- Navigate to other pages
- External links
- Download links

**Key attributes:**
- `variant` - `primary` | `secondary` | `default`

**Important notes:**
- Use for navigation, not actions (use rh-button for actions)
- Primary CTA uses brand red color
- Link goes inside the component: `<rh-cta><a href="...">Text</a></rh-cta>`

**Docs:** https://ux.redhat.com/elements/call-to-action/

---

### rh-chip
**Purpose:** Compact, removable items (tags)

**Use cases:**
- Filter selections
- Tags
- Dismissible labels

**Key attributes:**
- `removable` - Boolean, shows remove button

**Important notes:**
- Emits `remove` event when dismissed

**Docs:** https://ux.redhat.com/elements/chip/

---

### rh-switch
**Purpose:** Toggle between two states

**Use cases:**
- Enable/disable features
- On/off settings
- Boolean preferences

**Key attributes:**
- `checked` - Boolean, toggle state
- `disabled` - Boolean
- `label` - Accessible label

**Important notes:**
- Form-associated element
- Keyboard accessible (Space to toggle)

**Docs:** https://ux.redhat.com/elements/switch/

---

### rh-scheme-toggle
**Purpose:** Toggle between light and dark color schemes

**Use cases:**
- Theme switcher
- Color mode toggle

**Key attributes:**
- None (auto-detects and syncs with system preference)

**Important notes:**
- Persists preference to localStorage
- Icon-only button (includes accessible label)

**Docs:** https://ux.redhat.com/elements/scheme-toggle/

---

## Layout & Containers

### rh-card
**Purpose:** Flexible content container

**Use cases:**
- Group related content
- Content previews with actions
- Marketing content blocks

**Slots:**
- `header` - Card header
- `footer` - Card footer
- Default slot for body content

**Key attributes:**
- `color-palette` - Theme palette

**Important notes:**
- Can contain 0-2 CTAs
- Many patterns available (icon, image, logo, quote, etc.)
- See pattern library for variations

**Docs:** https://ux.redhat.com/elements/card/

---

### rh-tile
**Purpose:** Clickable container with single action

**Use cases:**
- Navigation cards
- Clickable content blocks
- Link cards

**Slots:**
- `image` - Optional image
- `headline` - Required, must contain exactly one `<a>`
- Default slot for description

**Key attributes:**
- `color-palette` - Theme palette
- `compact` - Boolean, reduced padding
- `bleed` - Boolean, image extends to edges

**Important notes:**
- Entire tile is clickable (unlike card)
- Must contain exactly one action (link in headline)
- Don't group multiple actions in a tile

**Docs:** https://ux.redhat.com/elements/tile/

---

### rh-surface
**Purpose:** Container that applies color palette to children

**Use cases:**
- Theme sections of a page
- Create visual hierarchy
- Apply consistent theming

**Key attributes:**
- `color-palette` - `lightest` | `lighter` | `light` | `dark` | `darker` | `darkest`

**Important notes:**
- Child elements automatically adapt to palette
- Provides theming context for descendants

**Docs:** https://ux.redhat.com/elements/surface/

---

## Navigation

### rh-breadcrumb
**Purpose:** Show navigation hierarchy

**Use cases:**
- Multi-level page navigation
- Show user's location in site structure

**Important notes:**
- Use `<a>` elements for links
- Last item (current page) should not be a link
- Automatically adds separators

**Docs:** https://ux.redhat.com/elements/breadcrumb/

---

### rh-tabs
**Purpose:** Organize content into tabs

**Use cases:**
- Group related content sections
- Reduce page scrolling
- Multi-view interfaces

**Child elements:**
- `rh-tab` (slot="tab") - Tab labels
- `rh-tab-panel` - Tab content

**Key attributes:**
- `box` - Boolean, box style tabs
- `vertical` - Boolean, vertical orientation

**Important notes:**
- First tab active by default
- Keyboard navigation with Arrow keys
- Use `active` attribute to set initial tab

**Docs:** https://ux.redhat.com/elements/tabs/

---

### rh-pagination
**Purpose:** Navigate through paginated content

**Use cases:**
- Table pagination
- Search results
- Content lists

**Key attributes:**
- `page` - Current page number (1-indexed)
- `total` - Total number of items
- `per-page` - Items per page

**Important notes:**
- Emits `page-change` event
- Automatically calculates total pages
- Includes first/last/prev/next controls

**Docs:** https://ux.redhat.com/elements/pagination/

---

### rh-jump-links
**Purpose:** Quick navigation to page sections

**Use cases:**
- Long pages with multiple sections
- Documentation navigation
- Anchor links

**Important notes:**
- Automatically finds headings or use explicit links
- Sticky positioning option
- Highlights active section

**Docs:** https://ux.redhat.com/elements/jump-links/

---

### rh-subnav
**Purpose:** Secondary navigation

**Use cases:**
- Section-specific navigation
- Subsection menus

**Important notes:**
- Use `<a>` elements for links
- Responsive (collapses to menu on mobile)

**Docs:** https://ux.redhat.com/elements/subnav/

---

### rh-navigation-primary
**Purpose:** Primary site navigation

**Use cases:**
- Main site header navigation
- Top-level navigation

**Important notes:**
- Complex component with many options
- Mobile-responsive

**Docs:** https://ux.redhat.com/elements/navigation-primary/

---

### rh-navigation-secondary
**Purpose:** Secondary navigation

**Use cases:**
- Sidebar navigation
- Section navigation

**Docs:** https://ux.redhat.com/elements/navigation-secondary/

---

### rh-navigation-vertical
**Purpose:** Vertical navigation menu

**Use cases:**
- Sidebar menus
- Vertical navigation lists

**Docs:** https://ux.redhat.com/elements/navigation-vertical/

---

### rh-back-to-top
**Purpose:** Scroll to top button

**Use cases:**
- Long pages
- Improve navigation

**Important notes:**
- Appears after scrolling down
- Fixed position (bottom right)

**Docs:** https://ux.redhat.com/elements/back-to-top/

---

### rh-skip-link
**Purpose:** Skip to main content (accessibility)

**Use cases:**
- Keyboard navigation
- Screen reader users

**Important notes:**
- Hidden until focused
- Should be first interactive element on page

**Docs:** https://ux.redhat.com/elements/skip-link/

---

## Feedback & Status

### rh-alert
**Purpose:** Display important messages

**Use cases:**
- Success/error messages
- Warnings
- Informational notices

**Slots:**
- `header` - Alert heading
- Default slot for message content

**Key attributes:**
- `state` - `default` | `info` | `success` | `warning` | `danger`
- `dismissable` - Boolean, shows close button

**Important notes:**
- Use appropriate state for message severity
- Emits `close` event when dismissed

**Docs:** https://ux.redhat.com/elements/alert/

---

### rh-badge
**Purpose:** Display counts or status indicators

**Use cases:**
- Notification counts
- Unread indicators
- Status badges

**Key attributes:**
- `number` - Number to display
- `threshold` - Max number before showing "99+"
- `state` - Visual variant

**Important notes:**
- Automatically formats large numbers

**Docs:** https://ux.redhat.com/elements/badge/

---

### rh-tooltip
**Purpose:** Contextual help on hover/focus

**Use cases:**
- Icon button labels
- Help text
- Additional information

**Key attributes:**
- `position` - `top` | `right` | `bottom` | `left`
- `trigger` - Element that triggers tooltip

**Important notes:**
- Use for supplementary info, not critical content
- Keyboard accessible (focus trigger to show)

**Docs:** https://ux.redhat.com/elements/tooltip/

---

### rh-spinner
**Purpose:** Loading indicator

**Use cases:**
- Loading states
- Processing indicators
- Async operations

**Key attributes:**
- `size` - `sm` | `md` | `lg` | `xl`

**Important notes:**
- Indeterminate (no progress percentage)
- Include accessible label for screen readers

**Docs:** https://ux.redhat.com/elements/spinner/

---

### rh-skeleton
**Purpose:** Loading placeholder

**Use cases:**
- Content loading states
- Skeleton screens

**Key attributes:**
- `shape` - `circle` | `square` | `text`
- `height`, `width` - Dimensions

**Important notes:**
- Animate while content loads
- Match layout of final content

**Docs:** https://ux.redhat.com/elements/skeleton/

---

### rh-tag
**Purpose:** Label or categorize content

**Use cases:**
- Content categories
- Labels
- Status indicators

**Key attributes:**
- `variant` - Visual style

**Important notes:**
- Not interactive (use rh-chip for removable tags)

**Docs:** https://ux.redhat.com/elements/tag/

---

### rh-announcement
**Purpose:** Display announcements or banners

**Use cases:**
- Site-wide announcements
- Temporary notices
- Promotional banners

**Important notes:**
- Prominent placement (top of page)
- Dismissable option

**Docs:** https://ux.redhat.com/elements/announcement/

---

### rh-stat
**Purpose:** Display statistics or metrics

**Use cases:**
- Key metrics
- Data visualization
- Dashboard statistics

**Slots:**
- `value` - Statistic value
- `description` - Description

**Docs:** https://ux.redhat.com/elements/stat/

---

### rh-site-status
**Purpose:** Display site/service status

**Use cases:**
- System status pages
- Service health indicators

**Docs:** https://ux.redhat.com/elements/site-status/

---

### rh-health-index
**Purpose:** Display health/grade indicator

**Use cases:**
- Health scores
- Ratings
- Grade indicators

**Key attributes:**
- `grade` - A through F

**Docs:** https://ux.redhat.com/elements/health-index/

---

## Content Display

### rh-accordion
**Purpose:** Expandable/collapsible content panels

**Use cases:**
- FAQs
- Expandable content sections
- Space-constrained layouts

**Child elements:**
- `rh-accordion-header` - Panel header (clickable)
- `rh-accordion-panel` - Panel content

**Key attributes:**
- `expanded-index` - Comma-separated list of expanded panel indices (0-based)
- `accents` - `inline` | `bottom`
- `large` - Boolean, larger size

**Important notes:**
- Keyboard navigation with Up/Down arrows
- Emits `expand` and `collapse` events
- Can have multiple panels open

**Docs:** https://ux.redhat.com/elements/accordion/

---

### rh-disclosure
**Purpose:** Show/hide content toggle

**Use cases:**
- Expandable sections
- Simple show/hide content
- Read more/less

**Child elements:**
- `rh-disclosure-toggle` - Clickable trigger
- `rh-disclosure-panel` - Content

**Important notes:**
- Simpler than accordion (single panel)
- Keyboard accessible

**Docs:** https://ux.redhat.com/elements/disclosure/

---

### rh-dialog
**Purpose:** Modal dialogs

**Use cases:**
- Confirmations
- Forms
- Content overlays

**Slots:**
- `header` - Dialog title
- `footer` - Action buttons
- Default slot for content

**Methods:**
- `show()` - Show as dialog
- `showModal()` - Show as modal (blocks interaction)
- `close()` - Close dialog

**Key attributes:**
- `variant` - Visual style

**Important notes:**
- Traps focus when modal
- Escape key closes dialog
- Emits `open` and `close` events

**Docs:** https://ux.redhat.com/elements/dialog/

---

### rh-popover
**Purpose:** Contextual popup content

**Use cases:**
- Contextual menus
- Popup content
- Floating UI

**Key attributes:**
- `trigger` - Element that triggers popover
- `position` - Placement

**Important notes:**
- Uses Floating UI for positioning
- Can be dismissable

**Docs:** https://ux.redhat.com/elements/popover/

---

### rh-table
**Purpose:** Data tables

**Use cases:**
- Tabular data
- Data grids
- Listings

**Important notes:**
- Use semantic `<table>` markup inside
- Requires lightdom CSS
- Responsive options available

**Docs:** https://ux.redhat.com/elements/table/

---

### rh-code-block
**Purpose:** Display formatted code

**Use cases:**
- Code examples
- Syntax highlighting
- Technical documentation

**Key attributes:**
- `highlight` - Language for syntax highlighting

**Important notes:**
- Includes copy button
- Supports many languages

**Docs:** https://ux.redhat.com/elements/code-block/

---

### rh-blockquote
**Purpose:** Display quotations

**Use cases:**
- Customer quotes
- Testimonials
- Citations

**Slots:**
- `author` - Quote author
- `cite` - Citation source

**Docs:** https://ux.redhat.com/elements/blockquote/

---

### rh-footnote
**Purpose:** Footnote references and content

**Use cases:**
- Citations
- Additional information
- Reference notes

**Docs:** https://ux.redhat.com/elements/footnote/

---

### rh-timestamp
**Purpose:** Display formatted dates/times

**Use cases:**
- Article dates
- Last updated timestamps
- Event times

**Key attributes:**
- `datetime` - ISO 8601 datetime
- `format` - Display format

**Important notes:**
- Automatically formats based on locale
- Updates relative times ("2 hours ago")

**Docs:** https://ux.redhat.com/elements/timestamp/

---

### rh-progress-stepper
**Purpose:** Multi-step progress indicator

**Use cases:**
- Multi-step forms
- Wizards
- Progress tracking

**Important notes:**
- Shows current step and progress

**Docs:** https://ux.redhat.com/elements/progress-stepper/

---

## Media

### rh-icon
**Purpose:** Display icons

**Use cases:**
- UI icons
- Decorative icons
- Icon buttons

**Key attributes:**
- `icon` - Icon name
- `set` - Icon set (`ui`, `microns`, `standard`)
- `size` - Icon size

**Important notes:**
- Icons from @rhds/icons package
- Use microns for smaller sizes
- Provide accessible labels for meaningful icons

**Docs:** https://ux.redhat.com/elements/icon/

---

### rh-avatar
**Purpose:** User avatars

**Use cases:**
- User profiles
- Comment authors
- Team member lists

**Key attributes:**
- `name` - User name (for initials)
- `src` - Image URL
- `size` - Avatar size

**Important notes:**
- Shows initials if no image
- Generates color from name

**Docs:** https://ux.redhat.com/elements/avatar/

---

### rh-video-embed
**Purpose:** Embed videos

**Use cases:**
- YouTube videos
- Vimeo videos
- Video content

**Key attributes:**
- `src` - Video URL (YouTube, Vimeo)

**Important notes:**
- Responsive aspect ratio
- Privacy-enhanced embeds

**Docs:** https://ux.redhat.com/elements/video-embed/

---

### rh-audio-player
**Purpose:** Audio player

**Use cases:**
- Podcasts
- Audio content
- Sound clips

**Key attributes:**
- `src` - Audio file URL

**Docs:** https://ux.redhat.com/elements/audio-player/

---

## Specialized

### rh-footer
**Purpose:** Site footer

**Use cases:**
- Page footer
- Site-wide footer

**Important notes:**
- Complex component with many sections
- Requires lightdom CSS
- Red Hat branding included

**Docs:** https://ux.redhat.com/elements/footer/

---

### rh-menu
**Purpose:** Menu container

**Use cases:**
- Dropdown menus
- Context menus

**Child elements:**
- `rh-menu-item`

**Docs:** https://ux.redhat.com/elements/menu/

---

### rh-menu-dropdown
**Purpose:** Dropdown menu button

**Use cases:**
- Action menus
- Option selectors

**Docs:** https://ux.redhat.com/elements/menu-dropdown/

---

### rh-navigation-link
**Purpose:** Navigation link with highlighting

**Use cases:**
- Active page indicators
- Navigation items

**Docs:** https://ux.redhat.com/elements/navigation-link/

---

## Component Usage Summary

**Most Commonly Used:**
- rh-button, rh-cta, rh-card, rh-alert, rh-icon, rh-tabs, rh-accordion

**Layout Essentials:**
- rh-card, rh-tile, rh-surface

**Navigation Must-Haves:**
- rh-breadcrumb, rh-tabs, rh-pagination

**Feedback & Status:**
- rh-alert, rh-spinner, rh-badge, rh-tooltip

**Forms & Interaction:**
- rh-button, rh-switch, rh-dialog

For detailed code examples, see [examples.md](examples.md).

For official documentation, visit https://ux.redhat.com/elements/
