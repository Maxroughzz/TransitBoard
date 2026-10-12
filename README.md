# TransitBoard — Smart Bus Tracking System

A browser-based prototype that shows where buses are, when they will reach your stop, which route they follow, and how long a trip will take. It runs entirely on the client with simulated, pre-assigned routes and timings, so there is nothing to install and no server to set up.

---

## Table of contents

- [The problem](#the-problem)
- [Features](#features)
- [Getting started](#getting-started)
- [How to use it](#how-to-use-it)
- [How it works](#how-it-works)
- [Project structure](#project-structure)
- [Customising routes](#customising-routes)
- [Tech stack](#tech-stack)
- [Testing](#testing)
- [Design Thinking background](#design-thinking-background)
- [How AI was used](#how-ai-was-used)
- [Limitations](#limitations)
- [Roadmap](#roadmap)

---

## The problem

On most campuses and in smaller cities, riders have no idea where their bus is. Research with commuters showed that the wait itself is not the main frustration. **Uncertainty is**: not knowing whether the bus is late, has already left, or is worth waiting for.

> Daily commuters need real-time visibility into a bus's location and arrival time, because uncertainty, not distance, is what makes waiting feel wasted.

TransitBoard answers three questions: *Where is my bus? When will it come? Will I make it?*

---

## Features

### Live tracking
- **Live route diagram** with colour-coded bus markers that move along the route and update every second.
- **Smooth, accurate positions**: markers are placed by interpolating between stops, and they reset cleanly when a bus starts a new lap.
- **Live fleet table** listing every bus in service with its next stop, ETA, lap progress, status and load. Click a row to open that bus's route.

### Arrival estimates
- **Per-stop arrival board** showing the next two buses at every stop.
- **Delay-aware ETAs**: simulated delays are included in every estimate, and late buses are highlighted.
- **Dynamic service notice** that appears when a bus runs about 2.5 minutes or more behind.
- **Network summary** with buses in service, on time, delayed and average delay.

### Trip planning
- Pick a route, a boarding stop and a destination.
- **Walking time** (0, 2, 5 or 10 min) so you are only offered buses you can actually catch.
- Shows **"Leave in X min"**, the recommended bus, its status and crowding level, ride duration and **arrival clock time**.
- **Backup option** if you miss the first bus.
- **Swap** button to reverse a trip, and the result **updates live** while the panel is open.

### Personalisation
- **Pinned stops ("My stops")**: star any stop to see its soonest bus across all routes.
- **Light and dark themes** with a toggle. Your choice is remembered, and the first visit follows your device setting.
- Last viewed route and pinned stops are remembered in your browser.

### Quality
- Responsive layout from desktop down to phone width.
- Accessibility: ARIA labels, visible keyboard focus and reduced-motion support.
- Single file, no dependencies, no build step.

---

## Getting started

**Run it locally**

```bash
git clone https://github.com/<Maxroughzz>/<TransitBoard>.git
cd <TransitBoard>
```

Then open `smart-bus-tracker-v2.html` in any modern browser (double-click it, or use a local server if you prefer):

```bash
python3 -m http.server 8000
# visit http://localhost:8000/smart-bus-tracker-v2.html
```

**Host it on GitHub Pages**

1. Rename `smart-bus-tracker-v2.html` to `index.html`.
2. In your repository go to **Settings → Pages**, choose the `main` branch and the root folder, then save.
3. Your site will be live at `https://<Maxroughzz>.github.io/<TransitBoard>/`.

> The page loads fonts from Google Fonts. If you are offline it falls back to system fonts and everything still works.

---

## How to use it

1. **Pick a route** from the tabs at the top. The route diagram shows each stop, its distance from the start and the next two arrivals.
2. **Watch the buses** move along the line. A red ring means that bus is running late.
3. **Plan a trip**: choose stops, set your walking time and press **Plan my trip**.
4. **Pin stops** you use often with the star icon. They appear under **My stops**.
5. **Check the fleet table** for every bus at a glance, and click any row to jump to its route.
6. **Switch theme** with the sun/moon button in the header.

---

## How it works

TransitBoard has three layers.

| Layer | Responsibility |
|---|---|
| **Data** | The `ROUTES` array: id, name, colour, loop time, bus count, length, and stops positioned from 0 to 1 along the route. |
| **Engine** | Pure functions that turn the current time into positions, delays, load and ETAs. No DOM access. |
| **Presentation** | Renders tabs, route diagram, planner, pinned stops and fleet table, refreshed once per second. |

Nothing about a bus is stored. Every value is recomputed from the current clock time, so the simulation always looks live and the same moment always gives the same result.

### Core formulas

For a route with loop time `P` seconds and `N` evenly spaced buses, bus `i` at time `T`:

```
position(i, T) = ((T + i × (P / N)) mod P) / P

delta = stopPosition − position(i, T)
if delta < 0:  delta += 1          # bus already passed, so it arrives next lap

ETA(i, stop, T) = delta × P + delay(i, T)
```

Trip planning finds buses that reach the boarding stop after your walking time, then adds the ride time between the two stops:

```
total = waitAtBoardingStop + rideTime
leaveIn = waitAtBoardingStop − walkingTime
```

Delay (0–4 min) and crowding (Low / Medium / High) are smooth functions of time, so they drift gradually and are repeatable. Delay is added to ETAs rather than to position, so markers never jump backwards.

---

## Project structure

```
.
├── smart-bus-tracker-v2.html   # the full app: HTML, CSS and JavaScript
├── Review2-Report.txt          # Review 2 submission report
├── screenshots/
│   ├── dark.png
│   └── light.png
└── README.md
```

Inside the HTML file, the script is split into an **ENGINE** section (pure logic) and a **UI** section (rendering and events), which keeps the logic easy to test.

---

## Customising routes

Edit the `ROUTES` array at the top of the script:

```js
{
  id: 'R7',
  name: 'Campus Loop',
  color: 'amber',        // amber | teal | violet
  period: 18 * 60,       // seconds for one full lap
  buses: 3,              // buses in service (evenly spaced)
  lengthKm: 5.4,
  seed: 0.4,             // varies delay and crowding patterns
  stops: [
    { name: 'Main Gate',      pos: 0    },
    { name: 'Library Circle', pos: 0.16 }
    // pos = how far along the route, from 0 to just under 1
  ]
}
```

The first stop should be at `pos: 0`. Add a new route by adding another object. To use a new colour, add `--c-<name>` and `--c-<name>-ink` variables for both themes in the CSS.

---

## Tech stack

- HTML5, CSS3 (custom properties for theming) and vanilla JavaScript
- `localStorage` for theme, last route and pinned stops
- Google Fonts: Archivo, Inter and IBM Plex Mono
- No frameworks, libraries or build tools

---

## Testing

- **Engine checks** across 4,000 time samples for every route and bus:
  - position stays within 0 to 1
  - delay stays within 0–240 s
  - ETAs are never negative
  - a bus just past a stop shows roughly one full lap (the wrap-around case)
  - trip totals equal wait plus ride
- **Browser check**: loaded in a headless browser in both themes, with the theme toggle, pinning and trip planner exercised and no script errors.
- **Manual review**: all generated formulas were traced by hand for before-stop, after-stop and at-stop cases.

---

## Design Thinking background

| Stage | Outcome |
|---|---|
| **Empathize** | Observed bus stops and interviewed commuters. Found that uncertainty, not delay, drives frustration, and that first-time riders feel it most. |
| **Define** | Framed the problem around certainty rather than GPS accuracy, which allowed a simulated, buildable solution. |
| **Ideate with AI** | Compared GPS, crowd-sourced, static and simulated approaches, and chose the simulated live system. |
| **Prototype** | TransitBoard v1, then this v2 upgrade. |
| **Validate** | Think-aloud sessions with three testers (v1). Feedback led to clearer route naming as a next step. |

---

## How AI was used

Claude was used as an ideation and build partner. Decisions were recorded as adopted, modified or rejected.

- **Adopted**: the time-based position engine and the combined wait + ride trip estimate.
- **Modified**: a larger route network was reduced to three routes, and route identity moved to colour-coding.
- **Rejected**: real GPS hardware, user accounts and multi-city support, all out of scope for this stage.

All generated logic was reviewed and tested before being accepted.

---

## Limitations

- Bus positions, delays and crowding are **simulated**, not live GPS, so real traffic and diversions are not reflected.
- Buses on a route are assumed to be evenly spaced.
- Three sample routes only, with no real map.
- Pinned stops and preferences live in one browser and do not sync across devices.

---

## Roadmap

- [ ] Real GPS feed in place of the simulation (the ETA logic stays the same)
- [ ] Rider alerts for delays and arrivals
- [ ] Driver companion app for location and status updates
- [ ] Route descriptions to help first-time riders
- [ ] Audio announcements and a larger-text mode
- [ ] Usability testing with a larger, more diverse group

---
