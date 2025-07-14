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
      <dgb-icon [icons]="dgbHeart" name="heart" iconStyle="fill"></dgb-icon>
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
      [icons]="dgbHeart"
      name="heart"
      iconStyle="duotone"
      size="48"
      color="red"
      secondaryColor="pink"
      opacity="0.8"
      secondaryOpacity="0.4"
    ></dgb-icon>
  `,
})
export class IconExampleComponent {}
```

> **Note:** Each DgbIcon component requires an `icons` prop with an array of possible icon definitions. The component will select the best icon based on the optional `name` and `iconStyle` props. If no `name` is provided, the component will use the first icon in the array of the corresponding style. If no `iconStyle` is provided, the component will use the first icon in the array.

### Use Icon Groups

Use `DgbIconGroup` when you need multiple icons to share the same properties:

```ts
import { Component } from "@angular/core";
import { DgbIconComponent, DgbIconGroupComponent } from "@/digibear-icons";
// Import from generated definitions
import { dgbHeart, dgbStar, dgbBell } from "./digibear-icon-definitions";

@Component({
  selector: "app-icon-group-example",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent],
  template: `
    <dgb-icon-group size="32" color="blue" opacity="0.9">
      <dgb-icon [icons]="dgbHeart" name="heart" iconStyle="fill"></dgb-icon>
      <!-- Final: size=32, color="blue", opacity=0.9, iconStyle="fill" -->

      <dgb-icon
        [icons]="dgbStar"
        name="star"
        iconStyle="duotone"
        secondaryColor="lightblue"
      ></dgb-icon>
      <!-- Final: size=32, color="blue", opacity=0.9, iconStyle="duotone", secondaryColor="lightblue" -->

      <dgb-icon
        [icons]="dgbBell"
        name="bell"
        iconStyle="line"
        size="24"
        color="red"
      ></dgb-icon>
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
import { dgbHeart, dgbStar } from "./digibear-icon-definitions";

@Component({
  selector: "app-nested-groups-example",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent],
  template: `
    <dgb-icon-group theme="danger" color="#ef4444">
      <dgb-icon [icons]="dgbHeart" name="heart" iconStyle="duotone"></dgb-icon>

      <!-- Nested group inherits danger theme but overrides color -->
      <div style="margin-top: 16px; padding: 8px; background-color: #fffbeb;">
        <dgb-icon-group color="orange">
          <dgb-icon
            [icons]="dgbStar"
            name="star"
            iconStyle="duotone"
          ></dgb-icon>
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
import { dgbHeart, dgbStar } from "./digibear-icon-definitions";

@Component({
  selector: "app-themed-icons",
  standalone: true,
  imports: [DgbIconComponent, DgbIconGroupComponent, DgbIconScopeComponent],
  template: `
    <dgb-icon-scope
      [themes]="myThemes"
      defaultTheme="primary"
      defaultVariant="default"
    >
      <!-- Using themed icons with default theme -->
      <dgb-icon [icons]="dgbHeart" name="heart" iconStyle="duotone"></dgb-icon>

      <!-- Using a specific theme via a group -->
      <div class="danger-section">
        <dgb-icon-group theme="danger">
          <dgb-icon
            [icons]="dgbHeart"
            name="heart"
            iconStyle="duotone"
          ></dgb-icon>
        </dgb-icon-group>
      </div>

      <!-- Using a specific variant -->
      <div class="highlight-section">
        <dgb-icon-group theme="primary" variant="highlight">
          <dgb-icon
            [icons]="dgbStar"
            name="star"
            iconStyle="duotone"
          ></dgb-icon>
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
}
```

## Component Properties

### DgbIconScope Component Inputs

- `themes` : Theme definitions for themed icons
- `defaultIconStyle` : Default icon style ("line", "fill", "duotone")
- `defaultTheme` : Default theme name to use from defined themes
- `defaultVariant` : Default variant name within the selected theme
- `defaultSize` : Default size for all icons (e.g., "24px", "24")
- `defaultColor` : Default color for all icons (leave undefined to inherit from CSS)
- `defaultOpacity` : Default opacity for primary color (0-1)
- `defaultSecondaryColor` : Default secondary color (leave undefined to use same as primary)
- `defaultSecondaryOpacity` : Default opacity for secondary color (0-1)

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

> **Note**: While explicit colors can be specified, it's generally recommended to use themes and variants or leave colors undefined to inherit the `currentColor` from the parent element. This ensures icons naturally match the text color they're displayed with.

### DgbIcon Component Inputs

- `icons` : Array of icon definitions (required)
- `name` : Icon name (required)
- `iconStyle` : Icon style - "line", "fill", or "duotone" (required)
- `theme` : Theme name to use from defined themes
- `variant` : Variant name within the selected theme
- `size` : Icon size (e.g., "24px", "24")
- `color` : Primary color (leave undefined to inherit from CSS)
- `opacity` : Opacity for primary color (0-1)
- `secondaryColor` : Secondary color for duotone icons
- `secondaryOpacity` : Opacity for secondary color (0-1)
- `class` : Additional CSS classes
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
