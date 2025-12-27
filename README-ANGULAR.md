# Digibear Icons for Angular

This guide will help you integrate Digibear Icons into your Angular application.

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

Then, scaffold the Angular components using the CLI:

```bash
# Install the CLI globally
npm install -g @digibearapp/icons-cli

# Scaffold Angular components
dgbear scaffold -f angular -v 1.0.0 -o ./src/app/digibear-icons
```

## Usage

### Import the Components

For standalone components (recommended in Angular 14+):

```ts
import { Component } from "@angular/core";
import {
  DgbIconComponent,
  DgbIconGroupComponent,
  DgbIconScopeComponent,
} from "@/digibear-icons";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent, DgbIconScopeComponent],
  template: ` <!-- Your template content --> `,
})
export class AppComponent {}
```

For NgModule-based approach:

```ts
import { NgModule } from "@angular/core";
import {
  DgbIconComponent,
  DgbIconGroupComponent,
  DgbIconScopeComponent,
} from "@/digibear-icons";
import { DigibearIconsModule } from "@/digibear-icons";

@NgModule({
  declarations: [],
  imports: [
    DigibearIconsModule,
    // other imports
  ],
  // ...
})
export class AppModule {}
```

### Set up the Icon Scope

The `DgbIconScope` component provides themes and default properties for all icons within its subtree:

```ts
import { Component } from "@angular/core";
import { DgbIconComponent, DgbIconScopeComponent } from "@/digibear-icons";
// RECOMMENDED: Import from generated definitions
import { dgbHeart } from "./digibear-icon-definitions";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [DgbIconComponent, DgbIconScopeComponent],
  template: `
    <dgb-icon-scope
      [themes]="themes"
      defaultTheme="primary"
      defaultVariant="default"
    >
      <!-- Your app content -->
      <dgb-icon name="heart" iconStyle="fill"></dgb-icon>
      <router-outlet></router-outlet>
    </dgb-icon-scope>
  `,
})
export class AppComponent {
  // Optional themes configuration
  themes = {
    primary: {
      default: {
        color: "var(--color-primary-600)",
        secondaryColor: "var(--color-primary-300)",
        secondaryOpacity: 0.3,
      },
    },
  };
}
```

### Using Icons

`dgb-icon` is the actual component you'll use in your templates:

```ts
import { Component } from "@angular/core";
import { DgbIconComponent } from "@/digibear-icons";
import { dgbHeart } from "./digibear-icon-definitions";

@Component({
  selector: "app-icon-example",
  standalone: true,
  imports: [DgbIconComponent],
  template: `
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
  `,
})
export class IconExampleComponent {}
```

> **Note:** Each DgbIcon component will get the icon based on the `name` (required) and `iconStyle` (optional) props. If no iconStyle is given, the icon will use the iconStyle from the closest DgbIconGroup or DgbIconScope or default to `duotone`. If the icon doesn't exist in the registry, the `questionBadge` icon will be rendered and a warning will be displayed in the console to register the icon.

### Use Icon Groups

Use `DgbIconGroup` when you need multiple icons to share the same properties:

```ts
import { Component } from "@angular/core";
import { DgbIconComponent, DgbIconGroupComponent } from "@/digibear-icons";

@Component({
  selector: "app-icon-group-example",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent],
  template: `
    <dgb-icon-group size="32" color="blue" opacity="0.9">
      <dgb-icon name="heart" iconStyle="fill"></dgb-icon>
      <!-- Final: size=32, color="blue", opacity=0.9, iconStyle="fill" -->

      <dgb-icon
        name="star"
        iconStyle="duotone"
        secondaryColor="lightblue"
      ></dgb-icon>
      <!-- Final: size=32, color="blue", opacity=0.9, iconStyle="duotone", secondaryColor="lightblue" -->

      <dgb-icon name="bell" iconStyle="line" size="24" color="red"></dgb-icon>
      <!-- Final: size=24, color="red", opacity=0.9, iconStyle="line" -->
    </dgb-icon-group>
  `,
})
export class IconGroupExampleComponent {}
```

### Nested Icon Groups

Groups can be nested to create complex icon hierarchies:

```ts
import { Component } from "@angular/core";
import { DgbIconComponent, DgbIconGroupComponent } from "@/digibear-icons";

@Component({
  selector: "app-nested-groups-example",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent],
  template: `
    <dgb-icon-group theme="danger" color="#ef4444">
      <dgb-icon name="heart" iconStyle="duotone"></dgb-icon>

      <!-- Nested group inherits danger theme but overrides color -->
      <div style="margin-top: 16px; padding: 8px; background-color: #fffbeb;">
        <dgb-icon-group color="orange">
          <dgb-icon name="star" iconStyle="duotone"></dgb-icon>
        </dgb-icon-group>
      </div>
    </dgb-icon-group>
  `,
})
export class NestedGroupsExampleComponent {}
```

### Using Themes

Themes provide a way to define consistent styling across your application:

```ts
import { Component } from "@angular/core";
import {
  DgbIconComponent,
  DgbIconGroupComponent,
  DgbIconScopeComponent,
} from "@/digibear-icons";
import { dgbIconRegistry } from "./dgb-registry";

@Component({
  selector: "app-themed-icons",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent, DgbIconScopeComponent],
  template: `
    <dgb-icon-scope
      [registry]="registry"
      [themes]="myThemes"
      defaultTheme="primary"
      defaultVariant="default"
    >
      <!-- Using themed icons with default theme -->
      <dgb-icon name="heart" iconStyle="duotone"></dgb-icon>

      <!-- Using a specific theme via a group -->
      <div class="danger-section">
        <dgb-icon-group theme="danger">
          <dgb-icon name="heart" iconStyle="duotone"></dgb-icon>
        </dgb-icon-group>
      </div>

      <!-- Using a specific variant -->
      <div class="highlight-section">
        <dgb-icon-group theme="primary" variant="highlight">
          <dgb-icon name="star" iconStyle="duotone"></dgb-icon>
        </dgb-icon-group>
      </div>
    </dgb-icon-scope>
  `,
})
export class ThemedIconsComponent {
  // Define your themes - can use CSS variables, hex, rgb, etc.
  myThemes = {
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
  registry = dgbIconRegistry;
}
```

## Component Properties

### DgbIconScope Component Inputs

- `registry`: A CLI generated registry containing all definitions for the icons you need. (required)
- `themes` : Theme definitions for themed icons
- `defaultIconStyle` : Default icon style ("line", "fill", "duotone")
- `defaultTheme` : Default theme name to use from defined themes
- `defaultVariant` : Default variant name within the selected theme
- `defaultSize` : Default size for all icons (e.g., "24px", "24")
- `defaultColor` : Default color for all icons (leave undefined to inherit from CSS)
- `defaultOpacity` : Default opacity for primary color (0-1)
- `defaultSecondaryColor` : Default secondary color (leave undefined to use same as primary)
- `defaultSecondaryOpacity` : Default opacity for secondary color (0-1)
- `defaultStrokeWidth`: Default stroke width for all icons (default : 1.5)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### DgbIconGroup Component Inputs

- `theme` : Theme name to use from defined themes
- `variant` : Variant name within the selected theme
- `iconStyle` : Icon style for all icons in the group
- `size` : Size for all icons in the group (e.g., "24px", "24")
- `color` : Primary color for all icons (leave undefined to inherit from CSS)
- `opacity` : Opacity for primary color (0-1)
- `secondaryColor` : Secondary color for duotone icons
- `secondaryOpacity` : Opacity for secondary color (0-1)
- `strokeWidth`: The stroke width for all icons in the group (default : 1.5)

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### DgbIcon Component Inputs

- `name` : Icon name (required)
- `iconStyle` : Icon style - "line", "fill", or "duotone" (required)
- `theme` : Theme name to use from defined themes
- `variant` : Variant name within the selected theme
- `size` : Icon size (e.g., "24px", "24")
- `color` : Primary color (leave undefined to inherit from CSS)
- `opacity` : Opacity for primary color (0-1)
- `secondaryColor` : Secondary color for duotone icons
- `secondaryOpacity` : Opacity for secondary color (0-1)
- `strokeWidth`: The stroke width of the icon (default : 1.5)
- `class` : Additional CSS classes
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
