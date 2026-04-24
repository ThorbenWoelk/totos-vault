---
created: 2026-04-02
last_edited: 2026-04-24
tags:
- ui
- ux
- frontend
- design
- vocabulary
- glossary
connections:
- '[[001 Knowledge Base/Programming/_index|Programming Index]]'
- '[[English]]'
- '[[Technical Vocabulary Ledger]]'
- '[[Rust]]'
ai_generated: true
human_approved: false
category:
- Knowledge Base
- Programming
---
# UI Design Vocabulary

A compact glossary of common UI and product design terms used in frontend work, design discussions, and design systems.

## Navigation & wayfinding

### Favicon
A **favicon** is the small icon shown in the browser tab, bookmarks, and sometimes app shortcuts. It helps users quickly recognize a site or app.

### Breadcrumb
A **breadcrumb** shows where the current page sits in a hierarchy, for example `Home / Settings / Billing`. It helps users understand context and navigate upward.

### Chevron
A **chevron** is a small arrow-like icon, often used to indicate direction, expansion, collapse, or “go deeper”. It usually communicates movement or hierarchy rather than a primary action.

### Kebab menu
A **kebab menu** is the three-dot vertical menu icon. It usually opens a list of secondary actions such as rename, duplicate, archive, or delete.

### Hamburger menu
A **hamburger menu** is the three-horizontal-lines icon. It is commonly used to open the main navigation drawer, especially on mobile.

## Layout & structure

### Bento box
A **bento box** layout organizes content into clearly separated tiles or panels of different sizes, similar to compartments in a lunch box. It is common in dashboards, landing pages, and AI product UIs.

### Card
A **card** is a self-contained UI container that groups related content and actions. Cards are useful when the interface needs modular, repeatable building blocks.

### Grid
A **grid** is a layout system that aligns content into rows and columns. It creates visual order and helps designers keep spacing and proportions consistent.

### Sidebar
A **sidebar** is a persistent vertical panel placed on the left or right side of the interface. It often contains navigation, filters, or supporting context.

### Drawer
A **drawer** is a panel that slides in from an edge of the screen. It is useful for navigation, filters, or temporary contextual actions without fully leaving the current page.

## Feedback & interaction

### Modal
A **modal** is a dialog that appears above the main interface and temporarily blocks background interaction until the user acts on it. It is best used for important confirmations, focused tasks, or decisions that need attention.

### Tooltip
A **tooltip** is a small floating hint that appears on hover, focus, or tap. It gives brief explanatory text without permanently taking up space.

### Toast
A **toast** is a lightweight temporary notification, often shown near a screen edge. It is useful for non-blocking feedback like “Saved”, “Copied”, or “Upload failed”.

### Popover
A **popover** is a floating surface attached to a trigger element. Compared with a tooltip, it can contain richer content such as buttons, forms, or multiple lines of explanation.

### Empty state
An **empty state** is the UI shown when no data exists yet, for example an empty inbox or an unused dashboard. Good empty states explain what is happening and suggest the next useful action.

## Input & control patterns

### CTA (Call to Action)
A **CTA** is the main action you want the user to take, such as `Sign up`, `Generate report`, or `Publish`. In UI design, CTA usually refers to the most visually emphasized action on a screen.

### Toggle
A **toggle** is a switch-like control for turning something on or off. It is best for immediate binary states, not for actions that require confirmation.

### Tabs
**Tabs** let users switch between parallel views or sections in the same context. They are useful when content belongs to the same level rather than a deeper navigation hierarchy.

### Accordion
An **accordion** is a vertically stacked set of expandable sections. It helps compress long content while still letting users reveal details on demand.

### Segmented control
A **segmented control** is a compact selector with multiple mutually exclusive options, often shown as connected buttons. It works well for switching views such as `Day / Week / Month`.

### Dropdown
A **dropdown** is a control that reveals a list of options when opened. It is useful when screen space is limited, but it hides choices until interaction.

### Combobox
A **combobox** combines text input with a selectable list of suggestions or options. It is useful for search, autocomplete, and large option sets.

### Checkbox
A **checkbox** represents an independent on/off choice. Multiple checkboxes can be selected at the same time.

### Radio button
A **radio button** represents one choice within a mutually exclusive group. Use it when exactly one option should be selected.

## Lists, collections & status patterns

### Badge
A **badge** is a small visual label used to show status, count, or category, such as `Beta`, `3`, or `Failed`. It is compact and meant for quick scanning.

### Chip
A **chip** is a small rounded UI element representing an option, filter, or selected item. Chips are often interactive in ways badges are not.

### Avatar
An **avatar** is a small visual representation of a user, team, or entity, often shown as an image or initials. It helps interfaces feel more identifiable and personal.

### Skeleton loader
A **skeleton loader** is a placeholder layout that mimics the shape of content while data is loading. It reduces perceived waiting better than a blank screen or spinner alone.

### Pagination
**Pagination** splits a large collection into separate pages. It helps with navigation, performance, and sense of place in long lists.

### Infinite scroll
**Infinite scroll** continuously loads more content as the user scrolls. It can feel smooth for discovery, but it often weakens orientation, footer access, and precise navigation.

## Visual hierarchy & usability

### Visual hierarchy
**Visual hierarchy** is the ordering of attention through size, color, contrast, spacing, and position. It helps users understand what matters first, second, and third.

### Affordance
An **affordance** is a cue that suggests how something can be used. A button looks pressable, a handle looks draggable, and a text field looks editable.

### Signifier
A **signifier** is the visible hint that communicates an affordance more explicitly. Icons, labels, borders, and hover states often act as signifiers.

### Progressive disclosure
**Progressive disclosure** means showing only the most relevant information or controls first, and revealing more detail only when needed. It reduces overwhelm while still keeping power available.

### Information scent
**Information scent** is the set of cues that helps users guess what will happen if they click or continue. Good labels and previews improve information scent.

### Above the fold
**Above the fold** refers to the part of the interface visible before scrolling. It still matters for first impressions, but good products should not assume everything important must fit there.

## Content & page patterns

### Hero section
A **hero section** is the prominent top area of a landing page, usually combining headline, subheadline, CTA, and supporting visual. It frames the page’s main value proposition.

### Command palette
A **command palette** is a searchable action launcher, often opened with a keyboard shortcut. It is useful for power users and large applications with many actions.

### Wizard
A **wizard** is a step-by-step guided flow for completing a complex task. It is helpful when users need structure, validation, or a clear sense of progress.

### Stepper
A **stepper** is the visual indicator of progress through a multi-step process. It shows where the user is, what comes next, and sometimes what has already been completed.

### Carousel
A **carousel** is a horizontally rotating set of items or panels. It can save space, but often hides content and reduces discoverability when overused.

## Common distinctions

### Modal vs drawer
- A **modal** interrupts the flow and demands attention.
- A **drawer** keeps the user closer to the current context and feels lighter.

### Tooltip vs popover
- A **tooltip** is brief, mostly explanatory, and non-interactive.
- A **popover** can hold richer, sometimes interactive content.

### Kebab menu vs CTA
- A **kebab menu** hides secondary actions.
- A **CTA** highlights the primary action.

## Why this vocabulary matters

Knowing these terms helps with:
- cleaner communication between design and engineering
- faster implementation discussions
- better component naming in code and design systems
- clearer critique of UI trade-offs

