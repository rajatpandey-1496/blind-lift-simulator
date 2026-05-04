# The Blind Lift Simulator

A browser-based prediction game where you watch an elevator travel up a 10-floor building and try to guess how many stops it will make — before it rides.

Originally conceived as a Python/pygame project, rebuilt as a fast, fully client-side React app with no backend required.

---
## 🎮 Play Now
👉 [Play Blind Lift Simulator](https://asset-manager--rajatpandey1496.replit.app/)

## How to Play

1. You see how many passengers are already aboard the elevator at floor 1
2. Set your prediction for the number of stops the elevator will make on the way up
3. Click **Lock In & Ride** — then watch
4. Score is based on how close your guess was to the actual stop count

### Scoring

| Difference | Rating |
|---|---|
| 0 | Perfect read 🎯 |
| 1 | So close 👌 |
| 2 | Not bad 🤔 |
| 3–4 | Off the mark 😬 |
| 5+ | Way off 💀 |

---

## Features

- **Animated elevator** rides floor by floor with eased motion
- **Passenger dots** shown inside the car (hidden on Hard mode)
- **Three difficulty modes**
  - Easy — slow speed, low boarding chance, all info visible
  - Normal — moderate speed and boarding chance
  - Hard — fast, higher boarding chance, passenger count and stops hidden during the ride
- **Daily Challenge** — everyone gets the exact same lift each day, generated from a date-based seed. One play per day, result saved locally
- **Session stats** — live accuracy %, current streak, best streak
- **All-time records** — total trips, best accuracy, best streak, per-difficulty trip counts, persisted in `localStorage`
- **Trip log** — shows every exit and boarding event after the ride
- **History mini-chart** — colour-coded bar chart of your last 10 trips

---

## Daily Challenge

Every day a new lift is generated using a seeded random number generator ([mulberry32](https://github.com/bryc/code/blob/master/jshash/PRNGs.md)) keyed to the current date. This means:

- Every player gets identical passengers and boarding events on the same day
- Results are stored in `localStorage` — refreshing the page keeps your result
- The card resets automatically at midnight for a fresh challenge

---

## Tech Stack

| Layer | Tech |
|---|---|
| Framework | React 18 + TypeScript |
| Build tool | Vite |
| Styling | Tailwind CSS v4 + tw-animate-css |
| State | React hooks only (no external state library) |
| Persistence | `localStorage` (no backend) |
| RNG | mulberry32 (seeded PRNG for daily mode) |

---

## Getting Started

### Prerequisites

- Node.js 18+
- pnpm

### Install

```bash
pnpm install
```

### Run (development)

```bash
PORT=5173 BASE_PATH=/ pnpm --filter @workspace/blind-lift run dev
```

Then open `http://localhost:5173` in your browser.

### Build

```bash
PORT=5173 BASE_PATH=/ pnpm --filter @workspace/blind-lift run build
```

### Simplifying for standalone use

If you pull just the `artifacts/blind-lift` folder and want to run it outside the monorepo, replace the `vite.config.ts` with a simpler version:

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";
import path from "path";

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "src") },
  },
});
```

Then run:

```bash
npm install
npm run dev
```

---

## Project Structure

```
artifacts/blind-lift/
├── src/
│   ├── pages/
│   │   └── Game.tsx        # All game logic and UI
│   ├── App.tsx             # Root component
│   ├── main.tsx            # Entry point
│   └── index.css           # Tailwind + custom animations
├── vite.config.ts
└── package.json
```

All game logic lives in a single file (`Game.tsx`) with no external game libraries — just React hooks and `requestAnimationFrame` for animation.

---

## localStorage Keys

| Key | Contents |
|---|---|
| `blindlift_v1` | All-time stats (trips, streaks, accuracy, per-difficulty counts) |
| `blindlift_daily_v1` | Today's daily challenge result (date, guess, actual, diff) |

---

## License

MIT
