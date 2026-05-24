# AGENTS.md — Obsidian Scan Sketch

## Quick start
```bash
npm install          # install deps
npm run dev          # esbuild watch (no typecheck)
npm run build        # tsc --noEmit then esbuild production
npm test             # vitest
npm run test:ui      # vitest --ui
npm run test:coverage
npx vitest test/<file>.test.ts              # single file
npx vitest -t "should initialize"            # single test case
npm run version      # bump version, update manifest.json + versions.json
```

## Build
- `main.js` is gitignored; only attached to GitHub releases (see `.github/workflows/release.yml`).
- Release: push a tag → CI builds + creates draft release with `main.js manifest.json styles.css`.
- `.npmrc` sets `tag-version-prefix=""` (no `v` prefix on `npm version`).

## Code conventions
- **Tabs** for indentation, **double quotes**, semicolons required.
- **Imports**: Obsidian API first, blank line, then local imports via path aliases (`Services/`, `UI/`).
- All canvas/Image/ImageData APIs are **mocked** in `test/setup.ts` (happy-dom lacks them). Write tests assuming mocked 2D context and async Image onload.
- Entry point: `main.ts` (`HandWrittenPlugin`) — lazy-loads `ScannerModal`.

## Project structure
- `Services/` — stateless pure functions, no class instances. Each file exports functions that operate on ImageData/Canvas types.
- `UI/Modals/` — Obsidian Modal subclasses. Use `await import()` for lazy loading.
- `UI/Components/` — UI widgets, not full modals.

## Path aliases (vitest.ts resolves these)
- `Services/` → `./Services/`
- `UI/` → `./UI/`
- These work in **vitest only** (not in esbuild, which bundles from `main.ts` directly).
