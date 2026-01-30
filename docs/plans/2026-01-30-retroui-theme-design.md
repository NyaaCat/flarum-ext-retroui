# Flarum RetroUI Theme Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a NeoBrutalism-styled Flarum theme extension that matches the homepage's RetroUI aesthetic.

**Architecture:** Pure LESS/CSS theme extension using Flarum's `Frontend` extender. No JavaScript required. All styles override Flarum's default components using CSS specificity.

**Tech Stack:** Flarum Extension (PHP extend.php), LESS preprocessor

---

## Task 1: Create Extension Boilerplate

**Files:**
- Create: `composer.json`
- Create: `extend.php`

**Step 1: Create composer.json**

```json
{
    "name": "phoenixlzx/flarum-ext-retroui",
    "description": "NeoBrutalism (RetroUI) theme for Flarum",
    "keywords": ["flarum", "theme", "retroui", "neobrutalism"],
    "type": "flarum-extension",
    "license": "MIT",
    "require": {
        "flarum/core": "^1.8.0"
    },
    "authors": [
        {
            "name": "phoenixlzx",
            "role": "Developer"
        }
    ],
    "autoload": {
        "psr-4": {
            "Phoenixlzx\\RetroUI\\": "src/"
        }
    },
    "extra": {
        "flarum-extension": {
            "title": "RetroUI Theme",
            "category": "theme"
        }
    }
}
```

**Step 2: Create extend.php**

```php
<?php

use Flarum\Extend;

return [
    (new Extend\Frontend('forum'))
        ->css(__DIR__.'/less/forum.less'),
];
```

**Step 3: Create directory structure**

```bash
mkdir -p less src
```

**Step 4: Commit**

```bash
git init
git add composer.json extend.php
git commit -m "feat: initialize flarum retroui theme extension"
```

---

## Task 2: Create Variables and Base Styles

**Files:**
- Create: `less/forum.less`
- Create: `less/variables.less`
- Create: `less/base.less`

**Step 1: Create less/variables.less**

```less
// RetroUI Theme Variables
// Based on Flarum's @primary-color and @secondary-color from admin config

// === Core Design Parameters ===
@retro-border-width: 2px;
@retro-border-color: @text-color;
@retro-border: @retro-border-width solid @retro-border-color;
@retro-border-light: 1px solid fade(@text-color, 20%);

@retro-shadow-offset: 4px;
@retro-shadow-default: @retro-shadow-offset @retro-shadow-offset 0 0 fade(@text-color, 35%);
@retro-shadow-hover: @retro-shadow-offset @retro-shadow-offset 0 0 @primary-color;
@retro-shadow-secondary: @retro-shadow-offset @retro-shadow-offset 0 0 @secondary-color;
@retro-shadow-large: 8px 8px 0 0 @text-color;

@retro-inset-shadow: inset 2px 2px 0 0 fade(@text-color, 10%);
@retro-inset-shadow-focus: inset 2px 2px 0 0 fade(@primary-color, 20%);

@retro-transition: 0.2s ease;

// === Override Flarum Defaults ===
@border-radius: 0;
```

**Step 2: Create less/base.less**

```less
// Base Global Styles
// Remove all border-radius, establish RetroUI foundation

// Global border-radius reset
* {
  border-radius: 0 !important;
}

// Ensure consistent box-sizing
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

**Step 3: Create less/forum.less (main entry)**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
```

**Step 4: Commit**

```bash
git add less/
git commit -m "feat: add variables and base styles"
```

---

## Task 3: Implement Button Styles

**Files:**
- Create: `less/buttons.less`
- Modify: `less/forum.less`

**Step 1: Create less/buttons.less**

```less
// Button Styles

.Button {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;
  transition: box-shadow @retro-transition;
  font-weight: 600;

  &:hover {
    box-shadow: @retro-shadow-hover;
  }

  &:focus {
    outline: none;
    box-shadow: @retro-shadow-hover;
  }

  // Primary button
  &.Button--primary {
    &:hover {
      box-shadow: @retro-shadow-secondary;
    }
  }

  // Icon-only buttons (flat style)
  &.Button--icon {
    border: none;
    box-shadow: none;
    background: transparent;

    &:hover {
      box-shadow: none;
      background: fade(@primary-color, 15%);
    }

    &:focus {
      box-shadow: none;
      background: fade(@primary-color, 15%);
    }
  }

  // Danger button
  &.Button--danger {
    &:hover {
      box-shadow: @retro-shadow-offset @retro-shadow-offset 0 0 @control-danger-color;
    }
  }

  // Link-style button
  &.Button--link {
    border: none;
    box-shadow: none;

    &:hover {
      box-shadow: none;
      text-decoration: underline;
    }
  }
}

// Button group
.ButtonGroup {
  .Button {
    margin-right: -@retro-border-width;

    &:last-child {
      margin-right: 0;
    }
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
```

**Step 3: Commit**

```bash
git add less/buttons.less less/forum.less
git commit -m "feat: add button styles"
```

---

## Task 4: Implement Header Styles

**Files:**
- Create: `less/header.less`
- Modify: `less/forum.less`

**Step 1: Create less/header.less**

```less
// Header / Navigation Styles

.Header {
  border-bottom: @retro-border;
  box-shadow: none;
}

.Header-title {
  font-weight: 700;
}

// Header navigation buttons
.Header-controls {
  .Button {
    border: @retro-border;
    box-shadow: @retro-shadow-default;

    &:hover {
      box-shadow: @retro-shadow-hover;
    }
  }

  .Button--icon {
    border: none;
    box-shadow: none;

    &:hover {
      box-shadow: none;
      background: fade(@primary-color, 15%);
    }
  }
}

// Search box
.Search-input {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-inset-shadow;
  transition: box-shadow @retro-transition, border-color @retro-transition;

  &:focus {
    border-color: @primary-color;
    box-shadow: @retro-inset-shadow-focus;
    outline: none;
  }
}

// Search results dropdown
.Search-results {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;
  margin-top: 4px;
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
```

**Step 3: Commit**

```bash
git add less/header.less less/forum.less
git commit -m "feat: add header styles"
```

---

## Task 5: Implement Form Styles

**Files:**
- Create: `less/forms.less`
- Modify: `less/forum.less`

**Step 1: Create less/forms.less**

```less
// Form Input Styles

.FormControl {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-inset-shadow;
  transition: box-shadow @retro-transition, border-color @retro-transition;

  &:focus {
    border-color: @primary-color;
    box-shadow: @retro-inset-shadow-focus;
    outline: none;
  }

  &.has-error,
  &:invalid {
    border-color: @error-color;
    box-shadow: inset 2px 2px 0 0 fade(@error-color, 20%);
  }
}

// Textarea
textarea.FormControl {
  resize: vertical;
}

// Select dropdown (trigger button style)
.Select {
  .Select-input {
    border: @retro-border;
    border-radius: 0;
    box-shadow: @retro-shadow-default;
    transition: box-shadow @retro-transition;

    &:hover {
      box-shadow: @retro-shadow-hover;
    }
  }
}

// Checkbox and radio
.Checkbox {
  input[type="checkbox"],
  input[type="radio"] {
    border: @retro-border;
    border-radius: 0;
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
```

**Step 3: Commit**

```bash
git add less/forms.less less/forum.less
git commit -m "feat: add form input styles"
```

---

## Task 6: Implement Card and Discussion List Styles

**Files:**
- Create: `less/cards.less`
- Modify: `less/forum.less`

**Step 1: Create less/cards.less**

```less
// Card and Discussion List Styles

// Discussion list items - clean by default, show on hover
.DiscussionListItem {
  border: @retro-border-width solid transparent;
  border-radius: 0;
  box-shadow: none;
  transition: border-color @retro-transition, box-shadow @retro-transition;
  margin-bottom: 4px;

  &:hover {
    border-color: @retro-border-color;
    box-shadow: @retro-shadow-hover;
  }

  // Active/unread discussions
  &.active {
    background: fade(@primary-color, 8%);
  }
}

// Individual posts - static border and shadow
.Post {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;
  margin-bottom: 12px;
}

// Post header
.Post-header {
  border-bottom: @retro-border-light;
}

// Sidebar navigation
.Sidebar {
  .SideNav {
    border: @retro-border;
    border-radius: 0;
    box-shadow: @retro-shadow-default;
  }

  .SideNav-item {
    border-bottom: @retro-border-light;

    &:last-child {
      border-bottom: none;
    }

    &:hover {
      background: fade(@primary-color, 10%);
    }

    &.active {
      background: fade(@primary-color, 15%);
      font-weight: 600;
    }
  }
}

// Index page sidebar
.IndexPage-nav {
  .SideNav {
    border: @retro-border;
    box-shadow: @retro-shadow-default;
  }
}

// Tags (if flarum/tags extension is installed)
.TagTile {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;
  transition: box-shadow @retro-transition;

  &:hover {
    box-shadow: @retro-shadow-hover;
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
```

**Step 3: Commit**

```bash
git add less/cards.less less/forum.less
git commit -m "feat: add card and discussion list styles"
```

---

## Task 7: Implement Dropdown Styles

**Files:**
- Create: `less/dropdown.less`
- Modify: `less/forum.less`

**Step 1: Create less/dropdown.less**

```less
// Dropdown Menu Styles

.Dropdown-menu {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;
  background: @control-bg;
  margin-top: 4px;
}

.Dropdown-item {
  transition: background @retro-transition;

  &:hover {
    background: fade(@primary-color, 10%);
  }

  &.active {
    background: fade(@primary-color, 15%);
    font-weight: 600;
  }
}

// Dropdown separator
.Dropdown-separator {
  border-top: @retro-border-light;
}

// User dropdown in header
.SessionDropdown {
  .Dropdown-menu {
    border: @retro-border;
    box-shadow: @retro-shadow-default;
  }
}

// Notifications dropdown
.NotificationsDropdown {
  .Dropdown-menu {
    border: @retro-border;
    box-shadow: @retro-shadow-default;
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
@import "dropdown.less";
```

**Step 3: Commit**

```bash
git add less/dropdown.less less/forum.less
git commit -m "feat: add dropdown menu styles"
```

---

## Task 8: Implement Modal Styles

**Files:**
- Create: `less/modals.less`
- Modify: `less/forum.less`

**Step 1: Create less/modals.less**

```less
// Modal / Dialog Styles

// Modal backdrop
.Modal-backdrop {
  background: fade(#000, 50%);
}

// Modal container
.Modal {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-large;
  background: @control-bg;
}

// Modal header
.Modal-header {
  border-bottom: @retro-border;
  font-weight: 700;
}

// Modal body
.Modal-body {
  // Content area - no special styling needed
}

// Modal footer
.Modal-footer {
  border-top: @retro-border;
}

// Modal close button (flat icon style)
.Modal-close {
  border: none;
  box-shadow: none;
  background: transparent;

  &:hover {
    background: fade(@primary-color, 15%);
    box-shadow: none;
  }
}

// Alert modal specific
.Modal--alert {
  .Modal-header {
    font-weight: 700;
  }
}

// Sign up / Log in modals
.LogInModal,
.SignUpModal {
  .Modal {
    border: @retro-border;
    box-shadow: @retro-shadow-large;
  }

  .FormControl {
    border: @retro-border;
    box-shadow: @retro-inset-shadow;

    &:focus {
      border-color: @primary-color;
      box-shadow: @retro-inset-shadow-focus;
    }
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
@import "dropdown.less";
@import "modals.less";
```

**Step 3: Commit**

```bash
git add less/modals.less less/forum.less
git commit -m "feat: add modal styles"
```

---

## Task 9: Implement Alert Styles

**Files:**
- Create: `less/alerts.less`
- Modify: `less/forum.less`

**Step 1: Create less/alerts.less**

```less
// Alert / Notification Styles

.Alert {
  border: @retro-border;
  border-radius: 0;
  box-shadow: @retro-shadow-default;

  // Success alert
  &.Alert--success {
    background: @alert-success-bg;
    color: @alert-success-color;
    border-color: @alert-success-color;
    box-shadow: @retro-shadow-offset @retro-shadow-offset 0 0 fade(@alert-success-color, 50%);
  }

  // Error alert
  &.Alert--error {
    background: @alert-error-bg;
    color: @alert-error-color;
    border-color: @error-color;
    box-shadow: @retro-shadow-offset @retro-shadow-offset 0 0 fade(@error-color, 50%);
  }

  // Info alert (default)
  &.Alert--info {
    background: @alert-bg;
    color: @alert-color;
  }
}

// Alert dismiss button
.Alert-dismiss {
  border: none;
  box-shadow: none;
  background: transparent;

  &:hover {
    background: fade(@text-color, 10%);
    box-shadow: none;
  }
}

// Alerts container
.AlertManager {
  .Alert {
    margin-bottom: 8px;
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
@import "dropdown.less";
@import "modals.less";
@import "alerts.less";
```

**Step 3: Commit**

```bash
git add less/alerts.less less/forum.less
git commit -m "feat: add alert styles"
```

---

## Task 10: Implement Composer (Post Editor) Styles

**Files:**
- Create: `less/composer.less`
- Modify: `less/forum.less`

**Step 1: Create less/composer.less**

```less
// Composer (Post Editor) Styles

.Composer {
  border: @retro-border;
  border-radius: 0;
  box-shadow: none;
  background: @control-bg;
}

// Composer header
.Composer-header {
  border-bottom: @retro-border-light;
}

// Composer body
.Composer-body {
  .TextEditor {
    // Text editor container
  }

  .TextEditor-editor {
    border: @retro-border-light;
    border-radius: 0;
    box-shadow: none;

    &:focus {
      border-color: @primary-color;
      outline: none;
    }
  }
}

// Text editor toolbar
.TextEditor-toolbar {
  border-bottom: @retro-border-light;

  .Button {
    border: none;
    box-shadow: none;
    background: transparent;

    &:hover {
      background: fade(@primary-color, 15%);
      box-shadow: none;
    }
  }
}

// Composer controls (submit button area)
.Composer-controls {
  .Button {
    border: @retro-border;
    box-shadow: @retro-shadow-default;

    &:hover {
      box-shadow: @retro-shadow-hover;
    }

    &.Button--primary {
      &:hover {
        box-shadow: @retro-shadow-secondary;
      }
    }
  }
}

// Full screen composer
.Composer--fullScreen {
  border: @retro-border;
  box-shadow: @retro-shadow-default;
}

// Minimized composer
.Composer--minimized {
  border: @retro-border;
  box-shadow: @retro-shadow-default;
}

// Reply preview
.ComposerBody-preview {
  border: @retro-border-light;
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
@import "dropdown.less";
@import "modals.less";
@import "alerts.less";
@import "composer.less";
```

**Step 3: Commit**

```bash
git add less/composer.less less/forum.less
git commit -m "feat: add composer (post editor) styles"
```

---

## Task 11: Implement Dark Mode Styles

**Files:**
- Create: `less/dark.less`
- Modify: `less/forum.less`

**Step 1: Create less/dark.less**

```less
// Dark Mode Styles
// Applied when Flarum's @config-dark-mode is enabled

& when (@config-dark-mode) {

  // Adjusted border color for dark mode (use lighter borders)
  @retro-border-color-dark: fade(@text-color, 60%);
  @retro-shadow-default-dark: @retro-shadow-offset @retro-shadow-offset 0 0 fade(@text-color, 20%);
  @retro-shadow-large-dark: 8px 8px 0 0 fade(@text-color, 40%);

  // Buttons
  .Button {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-default-dark;

    &:hover {
      box-shadow: @retro-shadow-hover;  // Keep primary color on hover
    }

    &.Button--icon {
      border: none;
      box-shadow: none;
    }
  }

  // Header
  .Header {
    border-bottom-color: @retro-border-color-dark;
  }

  // Search input
  .Search-input {
    border-color: @retro-border-color-dark;
  }

  // Form controls
  .FormControl {
    border-color: @retro-border-color-dark;
  }

  // Posts
  .Post {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-default-dark;
  }

  // Discussion list items
  .DiscussionListItem {
    &:hover {
      border-color: @retro-border-color-dark;
      box-shadow: @retro-shadow-hover;
    }
  }

  // Sidebar
  .Sidebar .SideNav {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-default-dark;
  }

  // Dropdowns
  .Dropdown-menu {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-default-dark;
  }

  // Modals
  .Modal {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-large-dark;
  }

  .Modal-header {
    border-bottom-color: @retro-border-color-dark;
  }

  .Modal-footer {
    border-top-color: @retro-border-color-dark;
  }

  // Alerts
  .Alert {
    border-color: @retro-border-color-dark;
    box-shadow: @retro-shadow-default-dark;
  }

  // Composer
  .Composer {
    border-color: @retro-border-color-dark;
  }
}
```

**Step 2: Update less/forum.less**

```less
// RetroUI Theme for Flarum
// Main entry point - imports all modules

@import "variables.less";
@import "base.less";
@import "buttons.less";
@import "header.less";
@import "forms.less";
@import "cards.less";
@import "dropdown.less";
@import "modals.less";
@import "alerts.less";
@import "composer.less";
@import "dark.less";
```

**Step 3: Commit**

```bash
git add less/dark.less less/forum.less
git commit -m "feat: add dark mode styles"
```

---

## Task 12: Create README and Finalize

**Files:**
- Create: `README.md`
- Verify all files

**Step 1: Create README.md**

```markdown
# RetroUI Theme for Flarum

A NeoBrutalism-styled theme for Flarum forums.

## Features

- Sharp corners (no border-radius)
- Bold 2px borders
- 4px offset shadows with color transitions on hover
- Follows Flarum's admin color configuration
- Dark mode support

## Installation

```bash
composer require phoenixlzx/flarum-ext-retroui
```

Then enable the extension in your Flarum admin panel.

## Configuration

The theme uses your Flarum admin panel color settings:
- **Primary Color**: Used for hover shadow effects
- **Secondary Color**: Used for primary button hover shadows

## License

MIT
```

**Step 2: Verify directory structure**

```
flarum-ext-retroui/
├── composer.json
├── extend.php
├── README.md
├── less/
│   ├── forum.less
│   ├── variables.less
│   ├── base.less
│   ├── buttons.less
│   ├── header.less
│   ├── forms.less
│   ├── cards.less
│   ├── dropdown.less
│   ├── modals.less
│   ├── alerts.less
│   ├── composer.less
│   └── dark.less
├── src/
│   └── (empty - no PHP needed for CSS-only theme)
└── docs/
    └── plans/
        └── 2026-01-30-retroui-theme-design.md
```

**Step 3: Final commit**

```bash
git add README.md
git commit -m "docs: add README"
```

---

## Summary

| Task | Description |
|------|-------------|
| 1 | Extension boilerplate (composer.json, extend.php) |
| 2 | Variables and base styles |
| 3 | Button styles |
| 4 | Header styles |
| 5 | Form input styles |
| 6 | Card and discussion list styles |
| 7 | Dropdown menu styles |
| 8 | Modal styles |
| 9 | Alert styles |
| 10 | Composer (post editor) styles |
| 11 | Dark mode styles |
| 12 | README and finalize |
