# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal links hub website built with **Astro** and **TypeScript**. It serves as a centralized landing page showcasing social media profiles and featured links. The site is based on the [AstroLinkHub template](https://github.com/MarcosKlender/AstroLinkHub) and customized for Piers Karsenbarg's personal branding.

## Stack and Dependencies

- **Framework**: Astro ^6.0.8
- **Language**: TypeScript ^6.0.2
- **Package Manager**: pnpm (v9 as per workflows)
- **Node Version**: v22.16.0 (`.node-version` file); CI uses Node 24.x
- **Build Tool**: Astro's built-in compiler

The site uses minimal dependencies - essentially just Astro, TypeScript, and Sharp for image optimization.

## Project Structure

```
src/
├── pages/
│   └── index.astro              # Main landing page
├── layouts/
│   └── Layout.astro             # Root HTML layout with global styles
├── components/
│   ├── sections/                # Page sections
│   │   ├── Header.astro         # Profile header with image
│   │   ├── Socials.astro        # Social media links section
│   │   ├── Buttons.astro        # Action buttons
│   │   ├── Featured.astro       # Featured content cards (with background images)
│   │   └── Footer.astro         # Footer section
│   ├── Analytics.astro          # Analytics integration (onedollarstats)
│   └── Section.astro            # Generic section wrapper (max-width 700px)
├── icons/                       # SVG icon components (30x30, currentColor)
│   ├── BlueSky.astro
│   ├── Discord.astro
│   ├── Facebook.astro
│   ├── GitHub.astro
│   ├── Instagram.astro
│   ├── Link.astro
│   ├── LinkedIn.astro
│   ├── Mail.astro
│   ├── Map.astro
│   ├── Medium.astro
│   ├── Spotify.astro
│   ├── TikTok.astro
│   ├── Twitch.astro
│   ├── WhatsApp.astro
│   ├── X.astro
│   └── YouTube.astro
├── images/                      # Profile images and assets
│   └── File_000.jpeg            # Profile photo
└── env.d.ts                     # TypeScript environment types

index.json                       # Central configuration file (content & styling)
astro.config.mjs                 # Astro configuration (minimal, all defaults)
tsconfig.json                    # TypeScript configuration with path aliases
.node-version                    # Node version lock (v22.16.0)
```

## index.json Structure

All site content and styling is controlled by `index.json`. The top-level keys are:

- **`html`**: `lang`, `title`, `description`, `favicon`
- **`background`**: `firstColor`, `secondColor` (used as CSS gradient variables)
- **`header`**: `image`, `title`, `description`, `textColor`
- **`socials`**: Array of `{ network, url }` objects — supports 16 networks (see icons list above)
- **`featured`**: Array of `{ title, description, url, image }` objects for featured content cards
- **`buttons`**: Array of `{ icon, label, url, bgColor, textColor, borderRadius }` objects
- **`footer`**: `text`, `url`

## Key Architectural Patterns

**Content-Driven Configuration**: All dynamic content (title, description, social links, buttons, colors, etc.) is managed in `index.json` rather than hardcoded. The Layout and section components import and destructure data from this single configuration file. This makes it easy to maintain and update without touching components.

**Path Aliases**: TypeScript path aliases are configured:
- `@/` → `src/`
- `@json` → `index.json`

Always use these aliases in imports for cleaner, more maintainable code.

**Component Composition**: The page is built from reusable, composable sections. Each section (Header, Socials, Buttons, Featured) is independent and can be reordered or modified without affecting others. All sections are wrapped in `Section.astro` for consistent spacing.

**Global Styling**: All global styles are in `Layout.astro` using CSS variables for theme colors (`--firstColor`, `--secondColor`). Individual components scope their styles with `<style>` blocks. The site uses the Onest font from Google Fonts.

**Static Site Only**: No client-side JavaScript framework. There is no React, Vue, or Svelte. The `Analytics.astro` component loads its script with `is:inline` and `defer`.

## Common Commands

```bash
# Development
pnpm dev          # Start dev server at http://localhost:3000
pnpm start        # Alias for dev

# Building
pnpm build        # Run type check and build (this is what CI runs)
pnpm check        # Run Astro type checking only
pnpm preview      # Preview production build locally

# Other
pnpm astro        # Direct Astro CLI access (e.g., pnpm astro add)
```

## Continuous Integration

The `.github/workflows/pr.yaml` workflow runs on PRs (opened, reopened, edited, synchronize) and:
1. Checks out the code (`actions/checkout@v6`)
2. Sets up pnpm v9 (`pnpm/action-setup@v5.0.0`)
3. Sets up Node 24.x with pnpm cache
4. Installs dependencies (`pnpm install`)
5. Runs `pnpm check` (Astro type checking)
6. Runs `pnpm build` (full build with checks)

Both check and build must pass for a PR to be mergeable.

**Dependabot** (`.github/dependabot.yaml`) runs weekly on Sundays and groups updates into two categories: Astro packages and all other packages. It also monitors GitHub Actions versions.

## Development Workflow

1. Make changes to components, styling, or configuration
2. Run `pnpm dev` to test locally
3. For content changes, modify `index.json` — do not hardcode values in components
4. Run `pnpm check` to verify TypeScript is valid
5. Run `pnpm build` to ensure production build succeeds
6. Commit and push — CI will validate automatically

## Adding a New Social Network

1. Create a new SVG icon at `src/icons/<NetworkName>.astro` following the 30x30 / `currentColor` pattern
2. Add the network to the `SOCIAL_ICONS` lookup in `Socials.astro`
3. Add an entry to the `socials` array in `index.json`

## Important Notes

- **Configuration is centralized**: Always update `index.json` for content changes, not hardcoded values
- **TypeScript is strict**: The project extends `astro/tsconfigs/strict` — all type errors must be resolved
- **Image optimization**: Sharp is a dev dependency for image optimization during builds
- **No framework interactivity**: This is a static site — there is no client-side JavaScript framework (no React, Vue, etc.)
- **VSCode**: The recommended extension is `astro-build.astro-vscode` (see `.vscode/extensions.json`)
