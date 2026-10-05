# Base44 Dev Environment

## Project Overview

**Rotion** is a React component library that renders Notion databases and pages. It is published to npm as `rotion`. The repo contains:

- `src/exporter` — data-fetching functions that call the Notion API (requires `NOTION_TOKEN`)
- `src/ui` — React components (Gallery, Table, List, Page, Block components, etc.)
- `website/` — a Next.js demo site that uses the library (requires `NOTION_TOKEN` + page IDs)
- `.storybook/` — Storybook config for component development

## Running the Preview

The preview runs **Storybook** (not the Next.js website), because Storybook uses fixture data and does not require Notion API credentials.

```bash
docker compose -f docker-compose.base44.yml up -d
```

- Storybook listens on port 6006 inside the container, mapped to host port 3000.
- Dependencies install via `npm ci` on container startup (node_modules in a named volume).
- The `website/public` directory must exist (Storybook config references it as `staticDirs`); it is created as an empty dir if absent.

## Notion API Credentials

The `website/` Next.js demo and the `src/exporter` functions require `NOTION_TOKEN` (a Notion integration secret). This is NOT needed for Storybook. If you want to run the website instead, you would need to provide `NOTION_TOKEN` and the page/database IDs from `website/.env.local`.

## Key Commands

- `npm run story` — run Storybook locally (port 6006)
- `npm run build` — build the library (exporter + UI)
- `npm test` — run exporter tests with uvu
- `npm run build-story` — build static Storybook

## Quirks

- Storybook 9.0.4 warns about `@storybook/addon-mdx-gfm@8.6.14` incompatibility — cosmetic, does not break.
- The `website/public` directory is referenced by Storybook's `staticDirs` but doesn't exist in the repo; it must be present or Storybook fails to start.
