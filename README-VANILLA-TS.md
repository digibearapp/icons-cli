# Digibear Icons for Vanilla TypeScript

This guide will help you integrate Digibear Icons into your Vanilla TypeScript application using Web Components.

## Installation

First, install the required packages:

```bash
# Using npm
npm install @digibeardev/icons-core

# Using yarn
yarn add @digibeardev/icons-core

# Using pnpm
pnpm add @digibeardev/icons-core
```

## Usage

### Import the Components

```typescript
// Import the web components
import "@digibeardev/icons-core/components";

// Import the icon definitions
import { dgbHeart, dgbStar, dgbBell } from "./digibear-icon-definitions";
```

### Set up the Icon Scope

The `dgb-icon-scope` component provides themes and default properties for all icons within its subtree:

```html
<dgb-icon-scope
  id="icon-scope"
  default-theme="primary"
  default-variant="default"
  default-icon-style="duotone"
  default-size="32"
>
  <!-- Your app content -->
  <dgb-icon data-icon="heart"></dgb-icon>
</dgb-icon-scope>

<script>
  // Set up themes
  const scope = document.getElementById("icon-scope");
  scope.themes = {
    primary: {
      default: {
        color: "var(--color-primary-600, #3b82f6)",
        secondaryColor: "var(--color-primary-300, #93c5fd)",
        secondaryOpacity: 0.3,
      },
    },
  };

  // Initialize icons
  const heartIcon = document.querySelector('[data-icon="heart"]');
  heartIcon.icons = dgbHeart;
  heartIcon.name = "heart";
</script>
```

### Using Icons

`dgb-icon` is the actual component you'll use in your HTML:

```html
<dgb-icon data-icon="heart"></dgb-icon>

<script>
  // Initialize the icon
  const heartIcon = document.querySelector('[data-icon="heart"]');
  heartIcon.icons = dgbHeart;
  heartIcon.name = "heart";
  heartIcon.iconStyle = "duotone";
  heartIcon.size = 48;
  heartIcon.color = "red";
  heartIcon.secondaryColor = "pink";
  heartIcon.opacity = 0.8;
  heartIcon.secondaryOpacity = 0.4;
</script>
```

> **Note:** Each dgb-icon component requires an `icons` property with an array of possible icon definitions. The component will select the best icon based on the optional `name` and `iconStyle` properties. If no `name` is provided, the component will use the first icon in the array of the corresponding style. If no `iconStyle` is provided, the component will use the first icon in the array.

### Use Icon Groups

Use `dgb-icon-group` when you need multiple icons to share the same properties:

```html
<dgb-icon-group id="icon-group">
  <dgb-icon data-icon="heart"></dgb-icon>
  <dgb-icon data-icon="star"></dgb-icon>
  <dgb-icon data-icon="bell" color="red" size="24"></dgb-icon>
</dgb-icon-group>

<script>
  // Set group properties
  const group = document.getElementById("icon-group");
  group.size = 32;
  group.color = "blue";
  group.opacity = 0.9;

  // Initialize icons
  const icons = document.querySelectorAll("[data-icon]");
  icons.forEach((icon) => {
    const iconType = icon.getAttribute("data-icon");
    switch (iconType) {
      case "heart":
        icon.icons = dgbHeart;
        icon.name = "heart";
        icon.iconStyle = "fill";
        break;
      case "star":
        icon.icons = dgbStar;
        icon.name = "star";
        icon.iconStyle = "duotone";
        icon.secondaryColor = "lightblue";
        break;
      case "bell":
        icon.icons = dgbBell;
        icon.name = "bell";
        icon.iconStyle = "line";
        break;
    }
  });
</script>
```

### Nested Icon Groups

Groups can be nested to create complex icon hierarchies:

```html
<dgb-icon-group id="danger-group">
  <dgb-icon data-icon="heart"></dgb-icon>

  <div style="margin-top: 16px; padding: 8px; background-color: #fffbeb;">
    <dgb-icon-group id="warning-group">
      <dgb-icon data-icon="star"></dgb-icon>
    </dgb-icon-group>
  </div>
</dgb-icon-group>

<script>
  // Set up the danger group
  const dangerGroup = document.getElementById("danger-group");
  dangerGroup.theme = "danger";
  dangerGroup.color = "#ef4444";

  // Set up the warning group
  const warningGroup = document.getElementById("warning-group");
  warningGroup.theme = "warning";
  warningGroup.color = "orange";

  // Initialize icons
  const heartIcon = document.querySelector('[data-icon="heart"]');
  heartIcon.icons = dgbHeart;
  heartIcon.name = "heart";
  heartIcon.iconStyle = "duotone";

  const starIcon = document.querySelector('[data-icon="star"]');
  starIcon.icons = dgbStar;
  starIcon.name = "star";
  starIcon.iconStyle = "duotone";
</script>
```

### Using Themes

Themes provide a way to define consistent styling across your application:

```html
<dgb-icon-scope id="themed-scope">
  <dgb-icon data-icon="heart"></dgb-icon>

  <div class="danger-section">
    <dgb-icon-group id="danger-group">
      <dgb-icon data-icon="heart"></dgb-icon>
    </dgb-icon-group>
  </div>

  <div class="highlight-section">
    <dgb-icon-group id="highlight-group">
      <dgb-icon data-icon="star"></dgb-icon>
    </dgb-icon-group>
  </div>
</dgb-icon-scope>

<script>
  // Define your themes
  const myThemes = {
    primary: {
      default: {
        color: "var(--color-primary-600, #3b82f6)",
        secondaryColor: "var(--color-primary-300, #93c5fd)",
        secondaryOpacity: 0.3,
      },
      highlight: {
        color: "var(--color-primary-700, #1d4ed8)",
        secondaryColor: "var(--color-primary-400, #60a5fa)",
        secondaryOpacity: 0.4,
      },
    },
    danger: {
      default: {
        color: "#ef4444",
        secondaryColor: "oklch(0.82 0.12 15.6 / 0.3)",
      },
    },
  };

  // Set up the scope
  const scope = document.getElementById("themed-scope");
  scope.themes = myThemes;
  scope.defaultTheme = "primary";
  scope.defaultVariant = "default";

  // Set up the groups
  const dangerGroup = document.getElementById("danger-group");
  dangerGroup.theme = "danger";

  const highlightGroup = document.getElementById("highlight-group");
  highlightGroup.theme = "primary";
  highlightGroup.variant = "highlight";

  // Initialize icons
  const icons = document.querySelectorAll("[data-icon]");
  icons.forEach((icon) => {
    const iconType = icon.getAttribute("data-icon");
    switch (iconType) {
      case "heart":
        icon.icons = dgbHeart;
        icon.name = "heart";
        icon.iconStyle = "duotone";
        break;
      case "star":
        icon.icons = dgbStar;
        icon.name = "star";
        icon.iconStyle = "duotone";
        break;
    }
  });
</script>
```

## Component Properties

### dgb-icon-scope Element Properties

- `themes`: Theme definitions for themed icons
- `defaultIconStyle`: Default icon style ("line", "fill", "duotone")
- `defaultTheme`: Default theme name to use from defined themes
- `defaultVariant`: Default variant name within the selected theme
- `defaultSize`: Default size for all icons (e.g., 24)
- `defaultColor`: Default color for all icons (leave undefined to inherit from CSS)
- `defaultOpacity`: Default opacity for primary color (0-1)
- `defaultSecondaryColor`: Default secondary color (leave undefined to use same as primary)
- `defaultSecondaryOpacity`: Default opacity for secondary color (0-1)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### dgb-icon-group Element Properties

- `theme`: Theme name to use from defined themes
- `variant`: Variant name within the selected theme
- `iconStyle`: Icon style for all icons in the group
- `size`: Size for all icons in the group (e.g., 24)
- `color`: Primary color for all icons (leave undefined to inherit from CSS)
- `opacity`: Opacity for primary color (0-1)
- `secondaryColor`: Secondary color for duotone icons
- `secondaryOpacity`: Opacity for secondary color (0-1)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### dgb-icon Element Properties

- `icons`: Array of icon definitions (required)
- `name`: Icon name (optional)
- `iconStyle`: Icon style - "line", "fill", or "duotone" (optional)
- `theme`: Theme name to use from defined themes
- `variant`: Variant name within the selected theme
- `size`: Icon size (e.g., 24)
- `color`: Primary color (leave undefined to inherit from CSS)
- `opacity`: Opacity for primary color (0-1)
- `secondaryColor`: Secondary color for duotone icons
- `secondaryOpacity`: Opacity for secondary color (0-1)
- Any standard SVG attributes are also supported

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

## Custom Icons and Manifest

If you have custom icons defined in your config file, make sure to run the `merge-custom-icons` command before sharing or copying your manifest to another project:

```bash
dgbear merge-custom-icons
```

This ensures that all custom icons from your config are properly embedded in the manifest file, making it portable and ready to use in other projects.

When importing icons, it's recommended to use the CLI generated icon definitions file (e.g., `digibear-icon-definitions.ts`) rather than importing directly from `@digibeardev/icons-core`. This ensures you're using the exact icons specified in your manifest, including any custom icons. The generated file bundles all specified styles for each icon in arrays, making them easier to use.

## Best Practices

- **Property Inheritance Priority**: dgb-icon > dgb-icon-group > dgb-icon-scope
- While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.
- Use nested icon groups to create complex hierarchies of styling
- For deeply nested components, place a dgb-icon-group at the appropriate level in the hierarchy to apply consistent styling
- Initialize icons with their definitions before setting other properties
- Set themes on the scope element before initializing icons

<img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icon Logo" width="42" style="vertical-align: -.9rem;">Copyright© 2025 Digibear Icons
