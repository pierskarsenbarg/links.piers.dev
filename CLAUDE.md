# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal links hub website built with **Astro** and **TypeScript**. It serves as a centralized landing page showcasing social media profiles and featured links. The site is based on the [AstroLinkHub template](https://github.com/MarcosKlender/AstroLinkHub) and customized for Piers Karsenbarg's personal branding.

## Stack and Dependencies

- **Framework**: Astro 5.15.8
- **Language**: TypeScript 5.9.3
- **Package Manager**: pnpm (v9 as per workflows)
- **Node Version**: 20.x
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
│   │   ├── Buttons.astro        # Featured action buttons
│   │   └── Footer.astro         # Footer section
│   ├── Analytics.astro          # Analytics component
│   └── Section.astro            # Generic section wrapper
├── icons/                       # SVG icon components for social platforms
│   ├── GitHub.astro
│   ├── LinkedIn.astro
│   ├── X.astro
│   └── ... (other social platforms)
└── images/                      # Profile images and assets

index.json                        # Central configuration file (content & styling)
astro.config.mjs                 # Astro configuration
tsconfig.json                    # TypeScript configuration with path aliases
```

## Key Architectural Patterns

**Content-Driven Configuration**: All dynamic content (title, description, social links, buttons, colors, etc.) is managed in `index.json` rather than hardcoded. The Layout and section components import and destructure data from this single configuration file. This makes it easy to maintain and update without touching components.

**Path Aliases**: TypeScript path aliases are configured:
- `@/` → `src/`
- `@json` → `index.json`

These should be used in imports for cleaner, more maintainable code.

**Component Composition**: The page is built from reusable, composable sections. Each section (Header, Socials, Buttons) is independent and can be reordered or modified without affecting others.

**Global Styling**: All global styles are in `Layout.astro` using CSS variables for theme colors (`--firstColor`, `--secondColor`). Individual components scope their styles with `<style>` blocks.

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

The `.github/workflows/pr.yaml` workflow runs on all pull requests and:
1. Checks out the code
2. Installs dependencies with pnpm
3. Runs `pnpm check` (Astro type checking)
4. Runs `pnpm build` (full build with checks)

Both check and build must pass for a PR to be mergeable.

## Development Workflow

1. Make changes to components, styling, or configuration
2. Run `pnpm dev` to test locally
3. For content changes, modify `index.json`
4. Run `pnpm check` to verify TypeScript is valid
5. Run `pnpm build` to ensure production build succeeds
6. Commit and push - CI will validate automatically

## Important Notes

- **Configuration is centralized**: Always update `index.json` for content changes, not hardcoded values
- **TypeScript is strict**: The project extends `astro/tsconfigs/strict` - all type errors must be resolved
- **Image optimization**: Sharp is included for image optimization during builds
- **No framework interactivity**: This is a static site - there's no client-side JavaScript framework (no React, Vue, etc.)
