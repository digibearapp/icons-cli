# Digibear Icons for Solid

> ⚠️ IMPORTANT  
> Digibear Icons is proprietary software owned by Digibear.  
> This repository documents internal tooling used by Digibear.  
> Usage is restricted to Digibear-built products only.
>
> See LICENSE.md for full terms.

This guide will help you integrate Digibear Icons into your Solid application.

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

Then, scaffold the Solid components using the CLI:

```bash
# Install the CLI globally
npm install -g @digibearapp/icons-cli

# Scaffold Solid components
dgbear scaffold -f solid -v 1.0.0 -o ./src/components/digibear-icons
```

## Usage

### Set up the Icon Scope

The `DgbIconScope` component provides themes and default properties for all icons within its subtree:

```tsx
import { DgbIcon, DgbIconScope } from "./components/digibear-icons";
// RECOMMENDED: Import the generated registry
import { dgbIconRegistry } from "./dgb-registry";

function App() {
  // Optional themes configuration
  const themes = {
    primary: {
      default: {
        color: "var(--color-primary-600)",
        secondaryColor: "var(--color-primary-300)",
        secondaryOpacity: 0.3,
      },
    },
  };

  return (
    <DgbIconScope
      registry={dgbIconRegistry}
      themes={themes}
      defaultTheme="primary"
      defaultVariant="default"
    >
      {/* Your app content */}
      <DgbIcon name="heart" iconStyle="fill" />
    </DgbIconScope>
  );
}

export default App;
```

### Using Icons

`DgbIcon` is the actual component you'll use in your templates:

```tsx
import { DgbIcon } from "./components/digibear-icons";

function IconExample() {
  return (
    <DgbIcon
      name="heart"
      iconStyle="duotone"
      size={48}
      color="red"
      secondaryColor="pink"
      opacity={0.8}
      secondaryOpacity={0.4}
      strokeWidth={0.7}
    />
  );
}

export default IconExample;
```

> **Note:** Each DgbIcon component will get the icon based on the `name` (required) and `iconStyle` (optional) props. If no iconStyle is given, the icon will use the iconStyle from the closest DgbIconGroup or DgbIconScope or default to `duotone`. If the icon doesn't exist in the registry, the `questionBadge` icon will be rendered and a warning will be displayed in the console to register the icon.

### Use Icon Groups

Use `DgbIconGroup` when you need multiple icons to share the same properties:

```tsx
import { DgbIcon, DgbIconGroup } from "./components/digibear-icons";

function IconGroupExample() {
  return (
    <DgbIconGroup size={32} color="blue" opacity={0.9}>
      <DgbIcon name="heart" iconStyle="fill" />
      {/* Final: size=32, color="blue", opacity=0.9, iconStyle="fill" */}

      <DgbIcon name="star" iconStyle="duotone" secondaryColor="lightblue" />
      {/* Final: size=32, color="blue", opacity=0.9, iconStyle="duotone", secondaryColor="lightblue" */}

      <DgbIcon name="bell" iconStyle="line" size={24} color="red" />
      {/* Final: size=24, color="red", opacity=0.9, iconStyle="line" */}
    </DgbIconGroup>
  );
}

export default IconGroupExample;
```

### Nested Icon Groups

Groups can be nested to create complex icon hierarchies:

```tsx
import { DgbIcon, DgbIconGroup } from "./components/digibear-icons";

function NestedGroupsExample() {
  return (
    <DgbIconGroup theme="danger" color="#ef4444">
      <DgbIcon name="heart" iconStyle="duotone" />

      {/* Nested group inherits danger theme but overrides color */}
      <div
        style={{
          marginTop: "16px",
          padding: "8px",
          backgroundColor: "#fffbeb",
        }}
      >
        <DgbIconGroup color="orange">
          <DgbIcon name="star" iconStyle="duotone" />
        </DgbIconGroup>
      </div>
    </DgbIconGroup>
  );
}

export default NestedGroupsExample;
```

### Using Themes

Themes provide a way to define consistent styling across your application:

```tsx
import {
  DgbIcon,
  DgbIconGroup,
  DgbIconScope,
} from "./components/digibear-icons";
import { dgbIconRegistry } from "./dgb-registry";

function ThemedIconsExample() {
  // Define your themes - can use CSS variables, hex, rgb, etc.
  const myThemes = {
    primary: {
      default: {
        color: "var(--color-primary-600)",
        secondaryColor: "var(--color-primary-300)",
        secondaryOpacity: 0.3,
      },
      highlight: {
        color: "var(--color-primary-700)",
        secondaryColor: "var(--color-primary-400)",
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

  return (
    <DgbIconScope
      registry={dgbIconRegistry}
      themes={myThemes}
      defaultTheme="primary"
      defaultVariant="default"
    >
      {/* Using themed icons with default theme */}
      <DgbIcon name="heart" iconStyle="duotone" />

      {/* Using a specific theme via a group */}
      <div class="danger-section">
        <DgbIconGroup theme="danger">
          <DgbIcon name="heart" iconStyle="duotone" />
        </DgbIconGroup>
      </div>

      {/* Using a specific variant */}
      <div class="highlight-section">
        <DgbIconGroup theme="primary" variant="highlight">
          <DgbIcon name="star" iconStyle="duotone" />
        </DgbIconGroup>
      </div>
    </DgbIconScope>
  );
}

export default ThemedIconsExample;
```

## Component Properties

### DgbIconScope Component Props

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

### DgbIconGroup Component Props

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

### DgbIcon Component Props

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
- `class`: Additional CSS classes
- Any standard SVG attributes are also supported

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

## Custom Icons and Manifest

If you have custom icons defined in your config file, make sure to run the `merge-custom-icons` command before sharing or copying your manifest to another project:

```bash
dgbear merge-custom-icons
```

This ensures that all custom icons from your config are properly embedded in the manifest file, making it portable and ready to use in other projects.

## Best Practices

- **Property Inheritance Priority**: DgbIcon > DgbIconGroup > DgbIconScope
- While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.
- Use nested icon groups to create complex hierarchies of styling
- For deeply nested components, place a DgbIconGroup at the appropriate level in the hierarchy to apply consistent styling

<img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icon Logo" width="42" style="vertical-align: -.9rem;">Digibear Icons - 2025
