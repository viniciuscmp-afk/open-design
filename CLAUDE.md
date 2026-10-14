# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

`AGENTS.md` (imported below) is the single source of truth for this repository: workspace layout, boundaries, lifecycle, validation, and commands. Keep this file to Claude Code-specific supplements and do not duplicate `AGENTS.md` content here.

@AGENTS.md

# Claude Code supplements

## Layer guides are not auto-loaded

Only the root `AGENTS.md` is imported above. Before editing or reviewing anything under these directories, read the matching guide yourself:

- `apps/AGENTS.md`, plus `apps/daemon/AGENTS.md` for daemon work
- `packages/AGENTS.md`
- `tools/AGENTS.md`, plus `tools/pack/AGENTS.md` for packaged build, updater, or installer work
- `e2e/AGENTS.md`
- `.github/AGENTS.md`
- `skills/AGENTS.md`, `design-templates/AGENTS.md`

For the runtime topology (browser → Next.js web → `/api/*` rewrite → Express daemon → spawned agent CLI/ACP process), read `docs/architecture.md`. `CONTEXT.md` holds the project's domain vocabulary.

## Running a single test

There is no root `pnpm test`. Almost every workspace uses Vitest; `tools/dev` and `apps/landing-page` are the exceptions and use the `node:test` runner.

```bash
# Vitest workspaces (pass -c vitest.config.ts where the package's test script does)
pnpm --filter @open-design/daemon exec vitest run -c vitest.config.ts tests/<file>.test.ts
pnpm --filter @open-design/web exec vitest run -c vitest.config.ts tests/<path>.test.tsx
pnpm --filter @open-design/contracts exec vitest run tests/<file>.test.ts
pnpm --filter @open-design/daemon exec vitest run -c vitest.config.ts tests/<file>.test.ts -t '<test name>'

# tools/dev and apps/landing-page (run from that package's directory)
node --import tsx --test tests/<file>.test.ts

# e2e: Vitest smoke suites live in e2e/tests and e2e/specs, Playwright UI suites in e2e/ui
pnpm --filter @open-design/e2e exec vitest run -c vitest.config.ts tests/<path>.test.ts
pnpm --filter @open-design/e2e exec playwright test -c playwright.config.ts ui/<file>.test.ts
```

Daemon test files run serially (`fileParallelism: false`) because they mutate process env/PATH and bind real local servers, so the full daemon suite is slow; prefer focused files while iterating. e2e suites carry `[P0]`/`[P1]`/`[P2]` tags in test titles, selectable with `pnpm --filter @open-design/e2e test:p0` and `test:ui:p0`.

## Commits

The "Git commit policy" in `AGENTS.md` forbids co-author metadata. Do not append a `Co-Authored-By` trailer to commits in this repository, including the default Claude Code one.
