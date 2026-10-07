# 67

A small Vue 3 browser game where players build a score by pressing **6** followed by **7**. It includes keyboard controls, sound effects, and a browser-persisted high score.

## Overview

This is a Vite-powered Vue 3 single-page application. `App.vue` owns game state, scoring, audio, browser persistence, and keyboard input. `GameControls.vue` renders the playable buttons, while `ScoreBoard.vue` displays the current and high scores. Audio files are bundled from `src/assets/sounds/`.

A successful round is the sequence `6 → 7`; an invalid `7` resets the pending sequence. The high score is stored under `six-seven-high-score` in `localStorage`.

## Quick Start

### Prerequisites

- Node.js `^22.18.0` or `>=24.12.0`
- npm

### Installation & Setup

```sh
npm install
npm run dev
```

Open the local URL printed by Vite. No environment variables are currently required.

## Development Commands

| Task | Command | Description |
| --- | --- | --- |
| Dev | `npm run dev` | Start Vite with hot reload. |
| Build | `npm run build` | Type-check and create the production bundle. |
| Preview | `npm run preview` | Serve the production bundle locally. |
| Type-check | `npm run type-check` | Run `vue-tsc` against the project. |
| Lint | `npm run lint` | Run Oxlint and ESLint with auto-fix. |
| Format | `npm run format` | Format source files with Prettier. |

## How to Play

- Click **6** and **7**, or press the corresponding keyboard keys.
- Press **6**, then **7** to increase the score.
- The high score is restored on later visits in the same browser.
- Audio may require an initial user interaction because of browser autoplay policies.

## Project Structure

```text
.
├── public/                  # Static public assets
├── src/
│   ├── assets/sounds/       # Bundled sound effects
│   ├── components/          # GameControls and ScoreBoard
│   ├── App.vue              # Game state and input lifecycle
│   └── main.ts              # Vue bootstrap
├── index.html               # Vite HTML entrypoint
├── package.json             # Scripts and dependencies
├── vite.config.ts           # Vite plugin configuration
└── tsconfig*.json           # TypeScript configuration
```

## Contributing

Keep changes focused, preserve the existing Vue component boundaries, and run the following before opening a pull request:

```sh
npm run type-check
npm run lint
npm run build
```

For game logic or UI changes, also verify the `6 → 7` interaction and high-score behavior in a browser. Do not commit generated output, local environment files, or dependency directories.

## Architecture Notes

The app has no server, database, API, or automated test suite. Runtime persistence is intentionally limited to browser `localStorage` for this single-player game. The implementation in `src/` and scripts in `package.json` are authoritative.

## License

No license has been declared in the repository yet.

## AI Coding Guidance

See [`AGENTS.md`](./AGENTS.md) for operational constraints, file boundaries, and verification rules.

## Package Manager

Use npm because `package-lock.json` is checked in. The project uses Vue 3, Vite 8, TypeScript 6, and Tailwind CSS 4.

## Repository

This project is maintained in the `Dirga36/67` repository.
