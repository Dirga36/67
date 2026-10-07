# AGENTS.md — Operational Guidelines for AI Coding Agents

## Core Principles & System Invariants

- Preserve the existing Vue 3 single-page architecture unless the task explicitly requires a broader change.
- Keep game state, browser persistence, keyboard handling, and audio orchestration in `src/App.vue`.
- Keep presentational responsibilities in `src/components/`; components should communicate with typed props and emits.
- Never introduce secrets, server-only assumptions, or trusted data into this client-side app.
- Do not replace `localStorage` with a backend unless the task explicitly changes the product requirements.
- Preserve the `6 → 7` scoring rule: only a pending `6` followed by `7` increments the score.

## Tech Stack & Tooling Specification

- **Framework:** Vue 3 with `<script setup lang="ts">`.
- **Build tool:** Vite 8.
- **Language:** TypeScript 6 with `vue-tsc`.
- **Styling:** Tailwind CSS 4 through `@tailwindcss/vite`, plus existing CSS in `src/assets/`.
- **Package manager:** npm only; keep `package-lock.json` synchronized with `package.json`.
- **Runtime:** Node.js `^22.18.0` or `>=24.12.0`.
- **Backend/database:** None. Do not add integration code for the current game without explicit requirements.

## Common Workflows & Command Execution

### 1. Inspect Before Editing

1. Read the relevant Vue component and its live callers.
2. Check `package.json` scripts and existing styling conventions.
3. Prefer the smallest change that preserves the current behavior.

### 2. Verification

Run the tightest relevant checks first:

```sh
npm run type-check
npm run lint
npm run build
```

For game logic, input, accessibility, or visible UI changes, also verify the behavior in the browser using the Vite preview. There is no configured automated test runner at present.

### 3. Development Server

```sh
npm run dev
```

Use the Vite URL shown by the development server. Do not add a second dev-server implementation.

## Code Conventions & Style Guide

- Use PascalCase for Vue component filenames and `camelCase` for functions and variables.
- Keep component props and emits explicitly typed.
- Prefer Vue refs and lifecycle hooks for reactive state and event listeners.
- Use semantic HTML and maintain accessible labels for interactive controls.
- Preserve `aria-live="polite"` on score updates unless there is a documented accessibility reason to change it.
- Use existing Tailwind utility patterns and avoid introducing a new styling system.
- Keep comments for non-obvious rationale only; let names and structure explain normal behavior.
- Use `Audio` only in browser execution paths; do not move browser APIs into build-time code.
- When handling local storage, parse defensively and keep the storage key stable unless a migration is intentional.

## File Map & Key Boundaries

| Path | Responsibility | Modifiability Rules |
| --- | --- | --- |
| `src/App.vue` | Game state, scoring, persistence, audio, keyboard lifecycle | Modify carefully; preserve scoring and mount/unmount behavior. |
| `src/components/GameControls.vue` | 6 and 7 buttons; emits typed input events | Safe to extend with controls; keep the event contract stable. |
| `src/components/ScoreBoard.vue` | Current and high-score presentation | Keep props typed and score announcements accessible. |
| `src/main.ts` | Vue bootstrap and global stylesheet import | Change only for app bootstrap concerns. |
| `src/assets/main.css` | Tailwind and global CSS entrypoint | Preserve Tailwind imports and existing global styles. |
| `src/assets/sounds/` | Bundled audio files | Add assets only when needed; update imports in `App.vue`. |
| `public/` | Static files served unchanged | Do not duplicate bundled assets here without a reason. |
| `vite.config.ts` | Vite plugins, aliases, and development tooling | Preserve Vue, JSX, devtools, and Tailwind plugins. |
| `package.json` | Scripts, dependency versions, and Node engine | Use npm and update the lockfile with dependency changes. |
| `README.md` | Human-facing setup and architecture documentation | Keep commands and structure accurate after changes. |
| `AGENTS.md` | AI-facing operational constraints | Update when architecture or workflows change. |

## Data & Security Boundaries

- `localStorage` is user-controlled and suitable only for the non-sensitive high score.
- Never place credentials, API keys, or private user data in source files or browser storage.
- There are currently no API routes, authentication flows, database queries, or environment variables.
- If a future task adds a backend or external service, document the integration and security model before implementing it.

## Change-Specific Guidance

### Game Logic

- Route button and keyboard input through the same `pressNumber` function.
- Keep random sound selection non-deterministic but bounded to the imported sound arrays.
- Reset the pending sequence after a completed round or invalid `7`.
- Update and persist the high score only after a successful `6 → 7` sequence.

### UI & Accessibility

- Keep controls keyboard accessible and labeled.
- Maintain responsive behavior for narrow and desktop viewports.
- Avoid replacing readable text with icon-only controls.
- Verify focus, interaction, score announcements, and layout when changing controls.

### Documentation

- Keep `README.md` concise and oriented toward human onboarding.
- Keep this file strict and operational; do not duplicate long product copy here.
- Use exact commands from `package.json` and do not document unsupported scripts.

## Pre-Commit Verification Checklist

1. Confirm only intended files changed.
2. Run `npm run type-check`.
3. Run `npm run lint`.
4. Run `npm run build`.
5. For user-visible or interaction changes, verify the app in a browser.
6. Confirm no `.env*`, `dist/`, `node_modules/`, logs, or generated artifacts were added.
7. Update `README.md` and/or `AGENTS.md` when the public workflow or architecture changes.

## Known Limitations

- No automated test suite is configured.
- The high score is local to the browser and is not shared across devices.
- Audio can be blocked until the user interacts with the page.
- The project has no declared license.

## Approved Package Workflow

Use npm:

```sh
npm install
```

When adding or updating a dependency, update both `package.json` and `package-lock.json`, then run the verification checklist.

## Completion Standard

A task is complete only when the implementation is focused, existing behavior is preserved, documentation remains accurate, and the relevant checks have passed or their limitation is clearly reported.

## Source of Truth

Treat the actual files under `src/` and the scripts in `package.json` as authoritative. Do not infer unsupported architecture from the starter template.

## Final Rule

Prefer a small, typed, accessible Vue change over a framework migration or new abstraction unless the user explicitly requests it.

## End

These guidelines apply to AI coding agents working in this repository.

## Documentation Pair

See `README.md` for the human-facing project overview.

## EOF

End of operational guidance.

## Final Marker

EOF

## Done

End.
