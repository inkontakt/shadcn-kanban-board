# AGENTS.md

## Cursor Cloud specific instructions

This is a single-service React Router v7 (SSR) app with Vite. No database, external API, or Docker is needed to run in dev mode.

### Services

| Service | Port | Command |
|---------|------|---------|
| Dev server | 3000 | `npm run dev` |

### Key commands

Commands are documented in `package.json` scripts. The important ones:

- **Lint**: `npm run lint`
- **Typecheck**: `npm run typecheck` (runs `react-router typegen` then `tsc`)
- **Unit tests**: `npm run test` (Vitest, verbose reporter)
- **E2E tests**: `npx playwright test --project=chromium` (only Chromium is installed in Cloud)
- **Build**: `npm run build`
- **Dev with mocks**: `npm run dev-with-mocks` (enables MSW client+server mocks)

### Non-obvious caveats

- Playwright is configured for chromium, firefox, and webkit, but only Chromium is installed in Cloud environments. Always pass `--project=chromium` when running E2E tests.
- The `playwright.config.ts` reuses the existing dev server when not in CI (`reuseExistingServer: true`). If you already have `npm run dev` running, Playwright will use it.
- Husky git hooks run `npm run lint && npm run typecheck` on pre-commit and `commitlint` on commit-msg. Commit messages must follow [Conventional Commits](https://www.conventionalcommits.org/) format (e.g., `feat: ...`, `fix: ...`, `chore: ...`).
- Prisma scripts exist in `package.json` but there is no Prisma schema or migrations directory; they are scaffolded for future use and not functional.
- The app uses client-side state only; no `.env` file or secrets are required for local development.
