# Digibear Icons for React

This guide will help you integrate Digibear Icons into your React application.

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

Then, scaffold the React components using the CLI:

```bash
# Install the CLI globally
npm install -g @digibearapp/icons-cli

# Scaffold React components
dgbear scaffold -f react -v 1.0.0 -o ./src/components/digibear-icons
```

## Usage

### Import the Components

```jsx
import React from "react";
import {
  DgbIcon,
  DgbIconGroup,
  DgbIconScope,
} from "./components/digibear-icons";
// RECOMMENDED: Import from generated definitions
import { dgbHeart } from "./digibear-icon-definitions";

function MyComponent() {
  return (
    <div>
      <DgbIcon icons={dgbHeart} name="heart" iconStyle="duotone" />
    </div>
  );
}

export default MyComponent;
```

### Set up the Icon Scope

The `DgbIconScope` component provides themes and default properties for all icons within its subtree:

```jsx
import React from "react";
import { DgbIcon, DgbIconScope } from "./components/digibear-icons";
// RECOMMENDED: Import from generated definitions
import { dgbHeart } from "./digibear-icon-definitions";

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
      themes={themes}
      defaultTheme="primary"
      defaultVariant="default"
    >
      {/* Your app content */}
      <DgbIcon icons={dgbHeart} name="heart" iconStyle="fill" />
    </DgbIconScope>
  );
}

export default App;
```

### Using Icons

`DgbIcon` is the actual component you'll use in your templates:

```jsx
import React from "react";
import { DgbIcon } from "./components/digibear-icons";
import { dgbHeart } from "./digibear-icon-definitions";

function IconExample() {
  return (
    <DgbIcon
      icons={dgbHeart}
      name="heart"
      iconStyle="duotone"
      size={48}
      color="red"
      secondaryColor="pink"
      opacity={0.8}
      secondaryOpacity={0.4}
    />
  );
}

export default IconExample;
```

> **Note:** Each DgbIcon component requires an `icons` prop with an array of possible icon definitions. The component will select the best icon based on the optional `name` and `iconStyle` props. If no `name` is provided, the component will use the first icon in the array of the corresponding style. If no `iconStyle` is provided, the component will use the first icon in the array.

### Use Icon Groups

Use `DgbIconGroup` when you need multiple icons to share the same properties:

```jsx
import React from "react";
import { DgbIcon, DgbIconGroup } from "./components/digibear-icons";
// Import from generated definitions
import { dgbHeart, dgbStar, dgbBell } from "./digibear-icon-definitions";

function IconGroupExample() {
  return (
    <DgbIconGroup size={32} color="blue" opacity={0.9}>
      <DgbIcon icons={dgbHeart} name="heart" iconStyle="fill" />
      {/* Final: size=32, color="blue", opacity=0.9, iconStyle="fill" */}

      <DgbIcon
        icons={dgbStar}
        name="star"
        iconStyle="duotone"
        secondaryColor="lightblue"
      />
      {/* Final: size=32, color="blue", opacity=0.9, iconStyle="duotone", secondaryColor="lightblue" */}

      <DgbIcon
        icons={dgbBell}
        name="bell"
        iconStyle="line"
        size={24}
        color="red"
      />
      {/* Final: size=24, color="red", opacity=0.9, iconStyle="line" */}
    </DgbIconGroup>
  );
}

export default IconGroupExample;
```

### Nested Icon Groups

Groups can be nested to create complex icon hierarchies:

```jsx
import React from "react";
import { DgbIcon, DgbIconGroup } from "./components/digibear-icons";
import { dgbHeart, dgbStar } from "./digibear-icon-definitions";

function NestedGroupsExample() {
  return (
    <DgbIconGroup theme="danger" color="#ef4444">
      <DgbIcon icons={dgbHeart} name="heart" iconStyle="duotone" />

      {/* Nested group inherits danger theme but overrides color */}
      <div
        style={{
          marginTop: "16px",
          padding: "8px",
          backgroundColor: "#fffbeb",
        }}
      >
        <DgbIconGroup color="orange">
          <DgbIcon icons={dgbStar} name="star" iconStyle="duotone" />
        </DgbIconGroup>
      </div>
    </DgbIconGroup>
  );
}

export default NestedGroupsExample;
```

### Using Themes

Themes provide a way to define consistent styling across your application:

```jsx
import React from "react";
import {
  DgbIcon,
  DgbIconGroup,
  DgbIconScope,
} from "./components/digibear-icons";
import { dgbHeart, dgbStar } from "./digibear-icon-definitions";

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
      themes={myThemes}
      defaultTheme="primary"
      defaultVariant="default"
    >
      {/* Using themed icons with default theme */}
      <DgbIcon icons={dgbHeart} name="heart" iconStyle="duotone" />

      {/* Using a specific theme via a group */}
      <div className="danger-section">
        <DgbIconGroup theme="danger">
          <DgbIcon icons={dgbHeart} name="heart" iconStyle="duotone" />
        </DgbIconGroup>
      </div>

      {/* Using a specific variant */}
      <div className="highlight-section">
        <DgbIconGroup theme="primary" variant="highlight">
          <DgbIcon icons={dgbStar} name="star" iconStyle="duotone" />
        </DgbIconGroup>
      </div>
    </DgbIconScope>
  );
}

export default ThemedIconsExample;
```

## Component Properties

### DgbIconScope Component Props

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

### DgbIconGroup Component Props

- `theme`: Theme name to use from defined themes
- `variant`: Variant name within the selected theme
- `iconStyle`: Icon style for all icons in the group
- `size`: Size for all icons in the group (e.g., 24)
- `color`: Primary color for all icons (leave undefined to inherit from CSS)
- `opacity`: Opacity for primary color (0-1)
- `secondaryColor`: Secondary color for duotone icons
- `secondaryOpacity`: Opacity for secondary color (0-1)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### DgbIcon Component Props

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
- `className`: Additional CSS classes
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

- **Property Inheritance Priority**: DgbIcon > DgbIconGroup > DgbIconScope
- While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.
- Use nested icon groups to create complex hierarchies of styling
- For deeply nested components, place a DgbIconGroup at the appropriate level in the hierarchy to apply consistent styling

<img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icon Logo" width="42" style="vertical-align: -.9rem;">Copyright© 2025 Digibear Icons
