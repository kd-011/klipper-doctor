---
slug: klipper-doctor-agents
name: klipper-doctor agent guide
repo: .
status: active
last_verified: 2026-11-22
---

# klipper-doctor: guide for coding agents

The doc index is docs/README.md. Read docs/ARCHITECTURE.md before changing structure.

## Commands

- Needs Node 18 or newer. `npm ci` installs the two dev dependencies (TypeScript and `@types/node`). There are no runtime dependencies. <!-- src: package.json#engines.node, package.json#devDependencies -->
- `npm run build` compiles `src/` to `dist/` with `tsc` and marks `dist/cli.js` executable. <!-- src: package.json#scripts.build -->
- `npm test` builds first (`pretest`), then runs Node's built-in runner over `test/*.test.mjs`. One file: `npm run build && node --test test/crash.test.mjs`. <!-- src: package.json#scripts.pretest, package.json#scripts.test -->
- `npm run check` typechecks both tsconfigs with no emit. The second one (`tsconfig.core.json`) has no Node types, so `src/core` cannot use Node APIs. <!-- src: package.json#scripts.check, tsconfig.core.json -->
- No linter or formatter is configured. <!-- src: package.json#scripts -->
- Run the CLI from source after a build: `node dist/cli.js test/fixtures/demo.log`. <!-- src: package.json#bin, README.md -->
- `npm run docs` rewrites the rule table in `docs/rules.md` from the rules' own metadata, and `node scripts/gen-rules-table.mjs --check` fails when it is stale. Build first. <!-- src: scripts/gen-rules-table.mjs, package.json#scripts.docs -->
- `npm run fixtures` rebuilds `test/fixtures/*.log` from `scripts/make-fixtures.mjs`. <!-- src: package.json#scripts.fixtures, scripts/make-fixtures.mjs:1-5 -->
- `npm run check-samples -- <klipper checkout>` lints every sample config in a Klipper checkout and counts errors, which would be false positives. It is not part of `npm test`. <!-- src: scripts/check-samples.mjs:1-8 -->
- `npm run bench` times `analyze()` and peak memory on generated logs. <!-- src: scripts/bench.mjs:1-4 -->
- `npm run assets` redraws `assets/*.png` from the engine's output and needs a headless Chromium (`CHROME_BIN`). <!-- src: scripts/render-assets.mjs:1-8 -->
- `node scripts/gen-command-errors.mjs <klipper checkout>` regenerates `src/core/rules/crash/commandErrorTable.ts`. <!-- src: scripts/gen-command-errors.mjs:1-3 -->
- `node scripts/release.mjs plan --version 0.3.0 --published 0.2.0 --release-exists false` is a dry run of the release decision. <!-- src: scripts/release.mjs:1-10, CONTRIBUTING.md -->

## Conventions

- `src/core/` takes text and returns a report, with no Node APIs. Only `src/cli.ts` reads files, stdin or the network. <!-- src: CONTRIBUTING.md, tsconfig.core.json, src/core/index.ts:386-398 -->
- No runtime dependencies. <!-- src: CONTRIBUTING.md, package.json#devDependencies -->
- A crash rule goes in `src/core/rules/crash/<subsystem>.ts` and a config rule in `src/core/rules/config/`. Each is listed in that folder's `index.ts`, and the order there only sets the order findings are listed in. <!-- src: src/core/rules/crash/index.ts:27-60, src/core/rules/config/index.ts:17-35 -->
- Every rule has an id, severity, title, detail, fix and a `docs` link to the Klipper docs page or source file it comes from. No source, no rule, and no invented thresholds. Copy message strings from the Klipper source, not from memory. <!-- src: src/core/types.ts:65-74, CONTRIBUTING.md -->
- A crash rule reads `ctx.cause`, `ctx.match`, `ctx.find` or `ctx.findAll`, which see only the lines before the first shutdown plus the reason Klipper gave. <!-- src: src/core/cause.ts:51-70, src/core/index.ts:102-149 -->
- Finding text is plain English: what happened, the likely cause, the fix. No em dashes, no marketing words. <!-- src: CONTRIBUTING.md -->
- Test fixtures are hand-built from the message formats in the Klipper source. Never use a real person's log. <!-- src: CONTRIBUTING.md, scripts/make-fixtures.mjs:1-5 -->
- A new rule needs a fixture in `scripts/make-fixtures.mjs`, an expectation in `test/crash.test.mjs` or `test/config.test.mjs`, a refreshed `docs/rules.md`, and `npm run check-samples` showing no errors when it reads the config. <!-- src: CONTRIBUTING.md -->
- The command line is described once in `src/options.ts`, and `--help` and the shell completions are both generated from it. <!-- src: src/options.ts:1-2, src/completions.ts -->
- `dist/` is committed. Commit the rebuilt files with any `src/` change. <!-- src: CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md -->
- A release is a pull request that raises the version: move the Unreleased notes in `CHANGELOG.md` under a `## [x.y.z] - date` heading, run `npm version <bump> --no-git-tag-version`, commit, merge. Do not tag by hand. <!-- src: CONTRIBUTING.md, .github/workflows/publish.yml:1-12 -->

## Traps

- CI fails when a rebuild changes `dist/`. Build with the TypeScript in `package-lock.json` (`npm ci`), not a global or stale one. <!-- src: .github/workflows/ci.yml:38-39, package.json#devDependencies.typescript -->
- `npm test` runs the build first, so running it can change tracked files under `dist/`. <!-- src: package.json#scripts.pretest -->
- The table in `docs/rules.md` between the `rules:start` and `rules:end` markers is generated. A hand edit is overwritten, and a stale table fails CI and `test/report.test.mjs`. <!-- src: scripts/gen-rules-table.mjs:12-17, test/report.test.mjs:67-72, .github/workflows/ci.yml:40-41 -->
- `src/core/rules/crash/commandErrorTable.ts` and `src/core/version.ts` are generated or written by scripts. Do not edit them by hand. <!-- src: src/core/rules/crash/commandErrorTable.ts:1, scripts/sync-version.mjs:1-2 -->
- Changing a rule's title, detail or fix wording fails `test/readme.test.mjs` until the quoted copy in `docs/examples.md` (and in `README.md` for `mcu-timer-too-close`) matches. The README must also stay under 700 words. <!-- src: test/readme.test.mjs:10-43 -->
- A merge to `main` that changes `package.json` runs the publish workflow. It publishes only when the version is new on npm, so a dependency bump publishes nothing. <!-- src: .github/workflows/publish.yml:20-24, CONTRIBUTING.md -->
- `test/kit.test.mjs` pins the maintenance kit: third-party actions pinned by full SHA, top-level `permissions` in every workflow, no workflow expanding PR or issue text, and no em dashes or private names in the kit files. <!-- src: test/kit.test.mjs:36-60, test/kit.test.mjs:138-151 -->
- `dossiers/klipper-doctor.md` is read by the same test: 5 to 15 claims, each citing a real file and a line range inside it. Editing a cited file can move those lines. <!-- src: test/kit.test.mjs:160-186, dossiers/klipper-doctor.md -->
- `--ignore` and `# klipper-doctor: ignore` hide config findings only. Crash findings and `unusable-input` are never hidden, and `--ignore` with an unknown or unhideable id exits 2. <!-- src: src/core/suppress.ts:3-5, src/cli.ts:95-102 -->
- The Moonraker fetch does not follow redirects, so the API key never goes to another address. Keep `redirect: 'manual'` and the key out of every message. <!-- src: src/cli.ts:156-160, src/cli.ts:104-111 -->
- The repo is public and its docs are link-checked weekly. Keep private names, hostnames and local paths out of every committed file. <!-- src: .github/workflows/upkeep.yml:21-33, test/kit.test.mjs:138-151 -->

## Unverified

- The npm trusted publisher entry on npmjs.com, the `NPM_TOKEN` secret and the ruleset applied on GitHub live outside the repo, so they could not be checked from the code.
- Windows is not covered by CI (ubuntu-latest only), so behaviour there is untested. <!-- src: .github/workflows/ci.yml:23-27 -->
