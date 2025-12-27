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
  <dgb-icon name="heart"></dgb-icon>
</dgb-icon-scope>

<script>
  // Import the web components and registry
  import "@digibeardev/icons-core/components";
  import { dgbIconRegistry } from "./dgb-registry";

  // Set up themes
  const scope = document.getElementById("icon-scope");
  scope.registry = dgbIconRegistry;
  scope.themes = {
    primary: {
      default: {
        color: "var(--color-primary-600, #3b82f6)",
        secondaryColor: "var(--color-primary-300, #93c5fd)",
        secondaryOpacity: 0.3,
      },
    },
  };
</script>
```

### Using Icons

`dgb-icon` is the actual component you'll use in your HTML:

```html
<dgb-icon
  name="heart"
  iconStyle="duotone"
  size="48"
  color="red"
  secondaryColor="pink"
  opacity="0.8"
  secondaryOpacity="0.4"
  strokeWidth="0.7"
></dgb-icon>
```

> **Note:** Each DgbIcon component will get the icon based on the `name` (required) and `iconStyle` (optional) props. If no iconStyle is given, the icon will use the iconStyle from the closest DgbIconGroup or DgbIconScope or default to `duotone`. If the icon doesn't exist in the registry, the `questionBadge` icon will be rendered and a warning will be displayed in the console to register the icon.

### Use Icon Groups

Use `dgb-icon-group` when you need multiple icons to share the same properties:

```html
<dgb-icon-group size="32" color="blue" opacity="0.9" id="icon-group">
  <dgb-icon name="heart" iconStyle="fill"></dgb-icon>
  <dgb-icon
    name="star"
    iconStyle="duotone"
    secondaryColor="lightblue"
  ></dgb-icon>
  <dgb-icon name="bell" iconStyle="line" color="red" size="24"></dgb-icon>
</dgb-icon-group>
```

### Nested Icon Groups

Groups can be nested to create complex icon hierarchies:

```html
<dgb-icon-group id="danger-group" theme="danger" color="#ef4444">
  <dgb-icon name="heart" iconStyle="duotone"></dgb-icon>

  <div style="margin-top: 16px; padding: 8px; background-color: #fffbeb;">
    <dgb-icon-group id="warning-group" theme="warning" color="orange">
      <dgb-icon name="star" iconStyle="duotone"></dgb-icon>
    </dgb-icon-group>
  </div>
</dgb-icon-group>
```

### Using Themes

Themes provide a way to define consistent styling across your application:

```html
<dgb-icon-scope
  id="themed-scope"
  defaultTheme="primary"
  defaultVariant="default"
>
  <dgb-icon name="heart"></dgb-icon>

  <div class="danger-section">
    <dgb-icon-group id="danger-group" theme="danger">
      <dgb-icon name="heart"></dgb-icon>
    </dgb-icon-group>
  </div>

  <div class="highlight-section" theme="primary" variant="highlight">
    <dgb-icon-group id="highlight-group">
      <dgb-icon name="star"></dgb-icon>
    </dgb-icon-group>
  </div>
</dgb-icon-scope>

<script>
  // Import the web components and registry
  import "@digibeardev/icons-core/components";
  import { dgbIconRegistry } from "./dgb-registry";

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
  scope.registry = dgbIconRegistry;
  scope.themes = myThemes;
</script>
```

## Component Properties

### dgb-icon-scope Element Properties

- `registry`: A CLI generated registry containing all definitions for the icons you need. (required)
- `themes`: Theme definitions for themed icons
- `defaultIconStyle`: Default icon style ("line", "fill", "duotone")
- `defaultTheme`: Default theme name to use from defined themes
- `defaultVariant`: Default variant name within the selected theme
- `defaultSize`: Default size for all icons (e.g., 24)
- `defaultColor`: Default color for all icons (leave undefined to inherit from CSS)
- `defaultOpacity`: Default opacity for primary color (0-1)
- `defaultSecondaryColor`: Default secondary color (leave undefined to use same as primary)
- `defaultSecondaryOpacity`: Default opacity for secondary color (0-1)
- `defaultStrokeWidth`: Default stroke width for all icons (default : 1.5)

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
- `strokeWidth`: The stroke width for all icons in the group (default : 1.5)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### dgb-icon Element Properties

- `name`: Icon name (optional)
- `iconStyle`: Icon style - "line", "fill", or "duotone" (optional)
- `theme`: Theme name to use from defined themes
- `variant`: Variant name within the selected theme
- `size`: Icon size (e.g., 24)
- `color`: Primary color (leave undefined to inherit from CSS)
- `opacity`: Opacity for primary color (0-1)
- `secondaryColor`: Secondary color for duotone icons
- `secondaryOpacity`: Opacity for secondary color (0-1)
- `strokeWidth`: The stroke width of the icon (default : 1.5)
- Any standard SVG attributes are also supported

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

## Custom Icons and Manifest

If you have custom icons defined in your config file, make sure to run the `merge-custom-icons` command before sharing or copying your manifest to another project:

```bash
dgbear merge-custom-icons
```

This ensures that all custom icons from your config are properly embedded in the manifest file, making it portable and ready to use in other projects.

## Best Practices

- **Property Inheritance Priority**: dgb-icon > dgb-icon-group > dgb-icon-scope
- While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.
- Use nested icon groups to create complex hierarchies of styling
- For deeply nested components, place a dgb-icon-group at the appropriate level in the hierarchy to apply consistent styling
- Initialize icons with their definitions before setting other properties
- Set themes on the scope element before initializing icons

<img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icon Logo" width="42" style="vertical-align: -.9rem;">Digibear Icons - 2025
