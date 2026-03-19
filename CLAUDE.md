# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`vite-plugin-html` is a Vite plugin that provides HTML minification and EJS template syntax support in `index.html`. It supports single-page applications (SPA) and multi-page applications (MPA).

## Monorepo Structure

```
packages/
├── core/           # Main plugin package (vite-plugin-html)
└── playground/     # Example projects
    ├── basic/      # SPA example
    ├── mpa/        # Multi-page application example
    └── custom-entry/
```

## Common Commands

```bash
pnpm install          # Install dependencies (runs stub setup automatically)
pnpm test             # Run all tests with Vitest (watch mode)
pnpm test -- --run    # Run tests once (CI mode)
pnpm test -- path     # Run a specific test file

# Build the core plugin package
cd packages/core && pnpm run build

pnpm run lint:eslint  # Lint code with ESLint

# Run playground dev servers
cd packages/playground/basic && pnpm run dev
cd packages/playground/mpa && pnpm run dev
```

## Architecture

The plugin is split into two main Vite plugins:

1. **`createPlugin`** (`htmlPlugin.ts`) - Core plugin handling:
   - `transformIndexHtml` - Processes HTML with EJS templating and injects entry scripts, data, and tags
   - `configResolved` - Loads environment variables via `loadEnv`
   - `configureServer` - Sets up history API fallback for dev server MPA support
   - `closeBundle` - Moves output HTML files and cleans up empty directories

2. **`createMinifyHtmlPlugin`** (`minifyHtml.ts`) - Post-build HTML minification using `html-minifier-terser`

Entry point (`index.ts`) returns an array of both plugins: `[createPlugin(userOptions), createMinifyHtmlPlugin(userOptions)]`

The `UserOptions` interface (defined in `typing.ts`) configures the plugin behavior including entry points, templates, inject data, and minification options.

## Key Dependencies

- `ejs` - Template rendering
- `html-minifier-terser` - HTML compression
- `node-html-parser` - HTML parsing for script removal
- `tinyglobby` - File globbing for output cleanup
- `connect-history-api-fallback` - Dev server MPA routing
- `dotenv` / `dotenv-expand` - Environment variable loading
