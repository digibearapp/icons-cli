<div align="center">
  <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icons Overview" width="128" style="vertical-align: -1.25rem;">
  <h1>Digibear Icons CLI</h1>
</div>

<div align="center">

#### <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-globe-west.svg" alt="Globe Icon" width="24" style="vertical-align: -0.42rem;"> [Digibear.dev](https://www.digibear.dev/icons)

</div>

<p align="center">
  <strong>Internal CLI tool for managing and generating Digibear Icons</strong>
</p>

<p align="center">
  <em>Proprietary tooling used exclusively for Digibear-built products.</em>
</p>

<p align="center">
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a> •
  <a href="#commands">Commands</a> •
  <a href="#configuration">Configuration</a> •
  <a href="#examples">Examples</a> •
  <a href="#supported-frameworks">Supported Frameworks</a> •
  <a href="#integration">Integration</a>
  <a href="#troubleshooting">Troubleshooting</a>
</p>

---

> ⚠️ **Important Notice**  
> Digibear Icons and all related tools are proprietary assets owned by Digibear.  
> This repository documents internal tooling used to generate icons for products and websites built by Digibear.
>
> Use, modification, or redistribution outside of Digibear-built projects is strictly prohibited.  
> Refer to the LICENSE file for full terms.

---

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-cloud-arrow-down.svg" alt="Cloud Icon With An Arrow Pointing Down" width="26" style="vertical-align: -0.36rem;"> Installation

You can install the Digibear Icons CLI globally using npm:

```bash
npm install -g @digibeardev/icons-cli
```

Or with yarn:

```bash
yarn global add @digibeardev/icons-cli
```

Or with pnpm:

```bash
pnpm add -g @digibeardev/icons-cli
```

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-book.svg" alt="Book Icon" width="26" style="vertical-align: -0.36rem;"> Usage

The Digibear Icons CLI provides a comprehensive set of tools to manage icon manifests, generate icon assets, and scaffold icon components for various frameworks.

### <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-pen-nib.svg" alt="Pen Nib Icon" width="26" style="vertical-align: -0.36rem;"> Basic Syntax

```bash
digibear-icons <command> [options]
```

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-terminal.svg" alt="Terminal Icon" width="26" style="vertical-align: -0.36rem;"> Commands

### `ls` / `list`

Lists all available frameworks and their supported versions.

```bash
digibear-icons list
```

### `gen` / `generate-icons`

Generates icon assets from a manifest file according to the configuration.

```bash
# Generate assets based on config file
dgbear gen

# Generate with specific config file
dgbear gen -c ./my-config.yaml

# Override SVG output path
dgbear gen --svg-output ./custom-svg-path
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)
- `--svg-output <path>` - Override SVG output path

### `scaffold`

Scaffolds Digibear Icons for a specific framework into your project.

```bash
# Basic scaffold with framework and version
dgbear scaffold -f react -v 1.0.0

# Scaffold with custom output directory
dgbear scaffold -f angular -v 1.0.0 -o ./src/components/icons

# Scaffold using config file
dgbear scaffold -c ./my-config.yaml
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)
- `-f, --framework <framework>` - Framework to scaffold
- `-v, --version <version>` - Framework version
- `-o, --output <path>` - Output directory

### `add`

Adds icons to the manifest file.

```bash
# Add an icon with specific styles
dgbear add heart duotone fill

# Add an icon with all styles
dgbear add star all

# Add an icon with styles from config plus additional style
dgbear add cactus +fill

# Add an icon with styles from config minus a style
# (NOTE: we're using a tilde "~" here not a minus "-" sign because those are reserved characters in bash)
dgbear add leaf ~duotone
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)

**Styles:**

- Use `all` to add all available styles
- Use `+style` to add a style to the config styles
- Use `~style` to remove a style from the config styles (NOTE: we're using a tilde "~" here not a minus "-" sign because those are reserved characters in bash)

### `rm` / `remove`

Removes icons from the manifest file.

```bash
dgbear rm <iconName> [styles...] [options]
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)

**Styles:**

- Use `all` to remove the icon completely
- Use specific styles to remove only those styles

### `clean`

Cleans and sorts the manifest file alphabetically.

```bash
dgbear clean [options]
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)

### `merge`

Merges multiple icon manifests into one.

```bash
dgbear merge <manifests...> [options]
```

**Options:**

- `-o, --output <path>` - Output manifest path
- `--with` - Include the manifest from default config in the merge
- `--no-validate` - Skip validation of icons and styles against bundled data

### `merge-custom-icons`

Adds or updates custom icons from config to manifest.

```bash
dgbear merge-custom-icons [options]
```

**Options:**

- `-c, --config <path>` - Path to config file (default: `./digibear-icons.yaml`)

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-cog-6.svg" alt="Gear Icon" width="26" style="vertical-align: -0.36rem;"> Configuration

The CLI uses a configuration file (default: `./digibear-icons.yaml` or `./digibear-icons.yml`) to manage your icon settings. You can create this file manually or let the CLI generate it for you when running commands.

### Configuration File Structure

```yaml
# Path to the icon manifest file
manifest: "./digibear-icon-pack.yaml"

# Default icon styles to use when not specified (duotone, fill, line)
styles:
  - duotone

# Output paths for different formats
outputs:
# 👇 ts output path (path type: file)
ts: "./digibear-icon-definitions.ts"
# 👇 android output path (path type: folder)
android: "./android-icons"
# 👇 flutter output path (path type: folder)
flutter: "./flutter-icons"
# 👇 js output path (path type: file)
js: "./digibear-icon-definitions.js"
# 👇 json output path (path type: file)
json: "./digibear-icon-definitions.json"
# 👇 swiftui output path (path type: folder)
swiftui: "./swiftui-icons"
# 👇 svg output path (path type: folder)
svg: "./svg-icons"
# Optional: Custom icons (only paths no SVG wrapping tag)
# customIcons:
#   - name: customIcon
#     iconStyle: duotone
#     content: >-
#       <path fill="#636F7E" opacity="0.16" d="M14.5,21 C17.538,21 20,18.09 20,14.5 C20,10.91 17.538,8 14.5,8 C13.6,8 12.75,8.256 12,8.709 C11.25,8.256 10.4,8 9.5,8 C6.463,8 4,10.91 4,14.5 C4,18.09 6.463,21 9.5,21 C10.4,21 11.25,20.744 12,20.291 C12.75,20.744 13.6,21 14.5,21 Z"/>
#       <path fill="#151E28" d="M9.5,7.25 C10.392,7.25 11.238,7.466 12,7.852 C12.763,7.466 13.609,7.25 14.5,7.25 C18.065,7.25 20.75,10.618 20.75,14.5 C20.75,18.382 18.065,21.75 14.5,21.75 C13.609,21.75 12.763,21.534 12,21.148 C11.238,21.534 10.392,21.75 9.5,21.75 C5.936,21.75 3.25,18.382 3.25,14.5 C3.25,10.618 5.936,7.25 9.5,7.25 Z M4.75,14.5 C4.75,17.798 6.989,20.25 9.5,20.25 C10.255,20.25 10.972,20.037 11.612,19.649 C11.851,19.505 12.15,19.505 12.388,19.649 C13.029,20.037 13.745,20.25 14.5,20.25 C17.011,20.25 19.25,17.798 19.25,14.5 C19.25,11.202 17.011,8.75 14.5,8.75 C13.745,8.75 13.029,8.964 12.388,9.351 C12.15,9.495 11.851,9.495 11.612,9.351 C10.972,8.964 10.255,8.75 9.5,8.75 C6.989,8.75 4.75,11.202 4.75,14.5 Z M16.901,2.669 C17.268,2.741 17.526,3.073 17.505,3.447 L17.495,3.634 C17.471,4.081 17.449,4.48 17.38,4.877 C17.297,5.349 17.151,5.805 16.884,6.386 C16.711,6.763 16.266,6.928 15.89,6.756 C15.513,6.583 15.348,6.138 15.521,5.761 C15.746,5.269 15.846,4.938 15.902,4.618 C15.932,4.45 15.95,4.282 15.965,4.086 C15.315,4.097 14.631,4.296 14.026,4.656 C13.216,5.138 12.62,5.861 12.424,6.676 C12.327,7.079 11.922,7.326 11.519,7.229 C11.116,7.132 10.869,6.727 10.966,6.324 C11.277,5.035 12.181,4.008 13.26,3.367 C14.337,2.726 15.662,2.426 16.901,2.669 Z"/>

# Optional: Scaffold configuration for framework integration
# scaffold:
#   # Framework to scaffold (flutter, swiftui, android, angular, qwik, react, solid, svelte, vue)
#   framework: "flutter"
#   # Version of the framework
#   version: "1.0.0"
#   # Output directory for scaffolded files
#   outputDir: "./digibear-icons"

# Optional: Theme configuration for styling icons
# themes:
#   light:
#     primary:
#       color: "#000000"
#       secondaryColor: "#666666"
#     secondary:
#       color: "#333333"
#       secondaryColor: "#999999"
#   dark:
#     primary:
#       color: "#ffffff"
#       secondaryColor: "#aaaaaa"
```

> **Tip:** When working with custom icons, always run `dgbear merge-custom-icons` after updating your config file to ensure the manifest contains all your custom icons. This is especially important before sharing your manifest with others or copying it to another project.

### Manifest Structure

The manifest file contains the icons and their available styles:

```yaml

# Standard icons section

icons:
heart: - duotone - fill
star: - duotone - line
cactus: - duotone

# Custom icons section with SVG content

customIcons:

- name: customIcon
  iconStyle: duotone
  content: >-
  <path fill="#636F7E" opacity="0.16" d="M14.5,21 C17.538,21 20,18.09 20,14.5 C20,10.91 17.538,8 14.5,8 C13.6,8 12.75,8.256 12,8.709 C11.25,8.256 10.4,8 9.5,8 C6.463,8 4,10.91 4,14.5 C4,18.09 6.463,21 9.5,21 C10.4,21 11.25,20.744 12,20.291 C12.75,20.744 13.6,21 14.5,21 Z"/>
  <path fill="#151E28" d="M9.5,7.25 C10.392,7.25 11.238,7.466 12,7.852 C12.763,7.466 13.609,7.25 14.5,7.25 C18.065,7.25 20.75,10.618 20.75,14.5 C20.75,18.382 18.065,21.75 14.5,21.75 C13.609,21.75 12.763,21.534 12,21.148 C11.238,21.534 10.392,21.75 9.5,21.75 C5.936,21.75 3.25,18.382 3.25,14.5 C3.25,10.618 5.936,7.25 9.5,7.25 Z M4.75,14.5 C4.75,17.798 6.989,20.25 9.5,20.25 C10.255,20.25 10.972,20.037 11.612,19.649 C11.851,19.505 12.15,19.505 12.388,19.649 C13.029,20.037 13.745,20.25 14.5,20.25 C17.011,20.25 19.25,17.798 19.25,14.5 C19.25,11.202 17.011,8.75 14.5,8.75 C13.745,8.75 13.029,8.964 12.388,9.351 C12.15,9.495 11.851,9.495 11.612,9.351 C10.972,8.964 10.255,8.75 9.5,8.75 C6.989,8.75 4.75,11.202 4.75,14.5 Z M16.901,2.669 C17.268,2.741 17.526,3.073 17.505,3.447 L17.495,3.634 C17.471,4.081 17.449,4.48 17.38,4.877 C17.297,5.349 17.151,5.805 16.884,6.386 C16.711,6.763 16.266,6.928 15.89,6.756 C15.513,6.583 15.348,6.138 15.521,5.761 C15.746,5.269 15.846,4.938 15.902,4.618 C15.932,4.45 15.95,4.282 15.965,4.086 C15.315,4.097 14.631,4.296 14.026,4.656 C13.216,5.138 12.62,5.861 12.424,6.676 C12.327,7.079 11.922,7.326 11.519,7.229 C11.116,7.132 10.869,6.727 10.966,6.324 C11.277,5.035 12.181,4.008 13.26,3.367 C14.337,2.726 15.662,2.426 16.901,2.669 Z"/>
```

### Custom Icons in Manifest

If you have custom icons defined in your config file, make sure to run the `merge-custom-icons` command before sharing or copying your manifest to another project:

```bash
dgbear merge-custom-icons
```

This ensures that all custom icons from your config are properly embedded in the manifest file, making it self-contained and ready to use in other projects.

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-map-3-folds.svg" alt="Map Icon" width="26" style="vertical-align: -0.36rem;"> Examples

### Managing Icon Manifests

```bash
# Add an icon with specific styles
dgbear add heart duotone fill

# Add an icon with all styles
dgbear add star all

# Remove specific styles from an icon
dgbear rm heart fill

# Remove an icon completely
dgbear rm star all

# Clean and sort the manifest
dgbear clean

# Merge multiple manifests
dgbear merge manifest1.yaml manifest2.yaml -o merged.yaml

# Merge with the config manifest
dgbear merge --with manifest1.yaml -o merged.yaml

# Add custom icons from config to manifest
dgbear merge-custom-icons
```

> **Important:** If your config contains custom icons and you plan to share or copy your manifest to another project, always run `dgbear merge-custom-icons` first to ensure all custom icons are embedded in the manifest file.

### Generating Icon Assets

```bash
# Generate assets according to config
dgbear gen

# Generate assets with a specific config file
dgbear gen -c ./my-config.yaml

# Override SVG output path
dgbear gen --svg-output ./custom-svg-path
```

### Scaffolding Framework Components

```bash
# Scaffold React icons
dgbear scaffold -f react -v 1.0.0 -o ./src/components/icons

# Scaffold Angular icons
dgbear scaffold -f angular -v 1.0.0 -o ./src/app/icons

# Scaffold Vue icons
dgbear scaffold -f vue -v 1.0.0 -o ./src/components/icons

# Scaffold Flutter icons
dgbear scaffold -f flutter -v 1.0.0 -o ./lib/icons
```

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-stack.svg" alt="Stack Icon" width="26" style="vertical-align: -0.36rem;"> Supported Frameworks

The Digibear Icons CLI currently supports the following frameworks:

### Web Frameworks

| Framework | Versions | Description                                   |
| --------- | -------- | --------------------------------------------- |
| React     | 1.0.0    | React components with hooks for customization |
| Angular   | 1.0.0    | Angular components and directives             |
| Vue       | 1.0.0    | Vue components with composables               |
| Qwik      | 1.0.0    | Qwik components with hooks                    |
| Svelte    | 1.0.0    | Svelte components                             |
| Solid     | 1.0.0    | SolidJs components                            |

### Platform Frameworks

| Framework | Versions | Description                                |
| --------- | -------- | ------------------------------------------ |
| Flutter   | 1.0.0    | Flutter painter and widget implementations |
| SwiftUI   | 1.0.0    | SwiftUI components for iOS/macOS           |
| Android   | 1.0.0    | ImageVector files for Android              |

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-question-badge.svg" alt="Question Mark Icon Inside A Badge" width="26" style="vertical-align: -0.36rem;"> What Gets Generated?

### Icon Assets

The `gen` command can generate various asset formats:

- **TypeScript/JavaScript/JSON**: Icon definitions for direct import
- **SVG**: Scalable Vector Graphics files
- **Flutter**: Dart code for Flutter apps (as CustomPainters)
- **SwiftUI**: Swift code for iOS/macOS/tvOS apps (as SwiftUI `Shape` definitions)
- **Android**: Kotlin code for Android/Jetpack Compose (as `ImageVector` definitions)

### Framework Components

When you scaffold a framework, the CLI will:

1. Create the necessary directory structure for the project
2. Copy all required components, hooks, and utilities
3. Set up the proper imports and exports

For example, scaffolding React icons will generate:

```digibear-icons/
├── components/
│ ├── dgb-icon-group.tsx
│ ├── dgb-icon-scope.tsx
│ ├── dgb-icon.tsx
│ └── ...
├── index.ts
└── README.md
```

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-info-badge.svg" alt="Info Icon Inside A Badge" width="26" style="vertical-align: -0.36rem;"> Integration

After scaffolding, you can integrate the icons into a project based on your framework of choice. Each framework has its own specific implementation details.

### Framework-Specific Documentation

For detailed integration instructions, please refer to the framework-specific README files:

| Framework                   | Documentation                                  |
| --------------------------- | ---------------------------------------------- |
| React                       | [README-react.md](./README-REACT.md)           |
| Vue                         | [README-vue.md](./README-VUE.md)               |
| Angular                     | [README-angular.md](./README-ANGULAR.md)       |
| Qwik                        | [README-qwik.md](./README-QWIK.md)             |
| Svelte                      | [README-svelte.md](./README-SVELTE.md)         |
| Solid                       | [README-solid.md](./README-SOLID.md)           |
| Vanilla TS (Web Components) | [README-vanilla-ts.md](./README-VANILLA-TS.md) |
| Flutter                     | [README-flutter.md](./README-FLUTTER.md)       |
| SwiftUI                     | [README-swiftui.md](./README-SWIFT-UI.md)      |
| Android                     | [README-android.md](./README-COMPOSE.md)       |

### Quick Start Examples

Here are some quick examples for popular frameworks:

#### React

```jsx
import React from "react";
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

#### Vue

```vue
<template>
  <DgbIconScope
    :registry="dgbIconRegistry"
    :themes="themes"
    defaultTheme="primary"
    defaultVariant="default"
  >
    <!-- Your app content -->
    <DgbIcon name="heart" iconStyle="fill" />
  </DgbIconScope>
</template>

<script setup>
import { DgbIcon, DgbIconScope } from "./components/digibear-icons";
// RECOMMENDED: Import the generated registry
import { dgbIconRegistry } from "./dgb-registry";

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
</script>
```

#### Web Components (Vanilla TS)

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

For more detailed examples and advanced usage, please refer to the framework-specific documentation.

## <img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/db-bug.svg" alt="Bug Icon" width="26" style="vertical-align: -0.36rem;"> Troubleshooting

### Common Issues

#### Command not found after installation

**Solution**: Make sure your global npm bin directory is in your PATH. You can check the location with:

```bash
npm bin -g
```

<hr />

#### Custom icons not appearing in generated assets

**Solution**: Make sure your custom icons are properly embedded in the manifest by running `dgbear merge-custom-icons` before generating assets.

<hr />

#### Permission errors when scaffolding

**Solution**: Make sure you have write permissions to the output directory. You might need to run the command with sudo on some systems.

<hr />

#### Incompatible framework version

**Solution**: Check the available versions for your framework with `dgbear list` and use a supported version.

<hr />

#### Missing icons in generated assets

**Solution**: Verify that the icons exist in your manifest and that they are available in the bundled data. Use the `--validate` option with the `merge` command to check for invalid icons.

<hr />

#### Error parsing config file

**Solution**: Make sure your config file is valid YAML or YML.

<hr />

<img src="https://gbiskldbvwxjvxtdxjko.supabase.co/storage/v1/object/public/brand/Logo-Sticker.svg" alt="Digibear Icon Logo" width="42" style="vertical-align: -.9rem;">Digibear Icons - 2025 - All rights reserved.
