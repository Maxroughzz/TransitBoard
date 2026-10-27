# TransitBoard — Smart Bus Tracking System

A browser-based smart bus tracker that shows where buses are, when they reach your stop, which route they follow and how long your trip will take. It runs on our **own time-based simulation** with pre-assigned routes and timings, so there is no server, no API key and nothing to install.

Built for **Project Better Tomorrow** (Design Thinking: Empathize, Define, Ideate with AI).

| Light theme | Dark theme |
|---|---|
| ![Light theme](screenshots/light.png) | ![Dark theme](screenshots/dark.png) |

## Contents

- [The problem](#the-problem)
- [Features](#features)
- [Getting started](#getting-started)
- [Using TransitBoard](#using-transitboard)
- [How the simulation works](#how-the-simulation-works)
- [Project structure](#project-structure)
- [Customising routes](#customising-routes)
- [Tech stack](#tech-stack)
- [Testing](#testing)
- [Design Thinking background](#design-thinking-background)
- [How AI was used](#how-ai-was-used)
- [Limitations](#limitations)
- [Future work](#future-work)

## The problem

On many campuses and in smaller cities, riders cannot see where their bus is. Our research found that the wait itself is not the biggest frustration. **Not knowing** is: is the bus late, has it already gone, is it worth waiting?

> Daily commuters need real-time visibility into a bus's location and arrival time, because uncertainty, not distance, is what makes waiting feel wasted.

TransitBoard answers three questions: where is my bus, when will it come, and will I make it?

## Features

**Tracking**
- Live network map with all three routes and buses gliding along them. Late buses get a red ring.
- Select a route from the tabs or by clicking it on the map. Stop names appear for the selected route.
- Live fleet table with each bus's next stop, arrival time, lap progress, status and seat availability.

**Arrival estimates**
- Next two buses at every stop, updated every second.
- Simulated delays are included in every estimate.
- A service notice appears when a bus is running about 2.5 minutes or more behind.
- Summary counts for buses running, on time and delayed.

**Trip planning**
- Choose a route, a boarding stop and a destination.
- Enter your walking time so you are only offered buses you can still reach.
- See "Leave in X min", the recommended bus, seat availability, ride time, arrival clock time and **estimated fare**.
- Backup bus if you miss the first one, a swap button, and results that refresh live.

**Convenience**
- **Stop search** across every route, with the next bus time for each match.
- **Pinned stops**: star a stop to keep it in "My stops" and get an **alert** when a bus is about two minutes away.
- **Time speed control** (real time, 10x, 60x) to watch a full lap in seconds. Useful for demos.
- **Light and dark themes**, remembered between visits, defaulting to your device setting.
- Route facts for each line: bus count, loop length, frequency and starting fare.

**Quality**
- Responsive from desktop to phone width.
- ARIA labels, keyboard focus outlines and reduced-motion support.
- One HTML file with no libraries and no build step.

## Getting started

**Run locally**

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

Open `smart-bus-tracker-v3.html` in a modern browser. If you prefer a local server:

```bash
python3 -m http.server 8000
# open http://localhost:8000/smart-bus-tracker-v3.html
```

**Host on GitHub Pages**

1. Rename `smart-bus-tracker-v3.html` to `index.html`.
2. Open **Settings > Pages**, pick the `main` branch and the root folder, then save.
3. Your site appears at `https://<your-username>.github.io/<your-repo>/`.

Fonts load from Google Fonts. Offline, the page falls back to system fonts and still works.

## Using TransitBoard

1. Pick a route from the tabs or click a route on the map.
2. Read the stop list for the next two buses at each stop.
3. Open **Plan a trip**, choose your stops and walking time, then select **Plan my trip**.
4. Star stops you use often. They show up under **My stops** and trigger alerts.
5. Use **Search a stop** to jump to any stop on any route.
6. Change **Time speed** to 10x or 60x to fast-forward the simulation.
7. Use the sun or moon button to switch theme.

## How the simulation works

The app has three layers.

| Layer | Role |
|---|---|
| **Data** | The `ROUTES` array: loop time, bus count, length, fare rate, map path and stops placed from 0 to 1 along the route. |
| **Engine** | Pure functions that turn the current time into positions, delays, seat levels, ETAs and trip plans. No DOM access. |
| **Presentation** | Draws the map, tabs, stop list, planner, pinned stops and fleet table, refreshed every second. |

Nothing about a bus is stored. Every value is recomputed from the clock, so the simulation always looks live and the same moment always gives the same result.

For a route with loop time `P` seconds and `N` evenly spaced buses, bus `i` at time `T`:

```
position(i, T) = ((T + i * (P / N)) mod P) / P

delta = stopPosition - position(i, T)
if delta < 0: delta += 1           # already passed, so it arrives next lap

ETA = delta * P + delay(i, T)
```

Trip planning keeps buses that reach your stop after your walking time, then adds the ride time:

```
leaveIn = waitAtStop - walkingTime
total   = waitAtStop + rideTime
fare    = max(5, round(5 + routeShare * routeKm * ratePerKm))
```

Map positions come from `getPointAtLength()` on each route's SVG path, so a bus at position `f` sits exactly `f` of the way along its line. Delay (0 to 4 min) and seat availability are smooth functions of time, so they drift gradually. Delay is added to ETAs, not to position, so buses never jump backwards. The speed control rescales the clock that feeds these functions.

## Project structure

```
.
├── smart-bus-tracker-v3.html   # the app: HTML, CSS and JavaScript
├── Final-Report.txt            # project completion report
├── screenshots/
│   ├── light.png
│   └── dark.png
└── README.md
```

Inside the HTML file, the script has an **ENGINE** section (logic) and a **UI** section (rendering and events).

## Customising routes

Edit the `ROUTES` array at the top of the script:

```js
{
  id: 'R7', name: 'Campus Loop', color: 'amber',  // amber | teal | violet
  period: 1080,    // seconds per lap
  buses: 3,        // evenly spaced
  km: 5.4,         // loop length
  rate: 1.5,       // fare per km
  seed: 0.4,       // varies delay and seat patterns
  path: 'M90 250C60 160 ... Z',   // closed SVG path for the map
  stops: [['Main Gate', 0], ['Library Circle', 0.16]]   // [name, position 0..1]
}
```

The first stop should be at `0`. To add a route, add another object. To add a colour, define `--<name>` and `--<name>-i` for both themes in the CSS.

## Tech stack

- HTML5, CSS3 (custom properties for theming), vanilla JavaScript
- Inline SVG for the network map
- `localStorage` for theme, last route and pinned stops
- Google Fonts: Fraunces, Inter and IBM Plex Mono

## Testing

- **Engine checks** over thousands of time samples per route and bus: positions stay between 0 and 1, delays stay within 0 to 240 seconds, ETAs are never negative, and a bus just past a stop shows about one full lap.
- **Browser checks** in light and dark themes: theme toggle, route switching, pinning, trip planning, stop search and the speed control ran with no script errors.
- **Hand review** of the position and wrap-around formulas.
- **User feedback** from three testers on the first prototype shaped the planner and route naming.

## Design Thinking background

| Stage | Outcome |
|---|---|
| Empathize | Observed stops and interviewed commuters. Uncertainty, not delay, drives frustration. |
| Define | Framed the problem around certainty rather than GPS accuracy. |
| Ideate with AI | Compared GPS, crowd-sourced, static and simulated options, and chose a simulated live system. |
| Prototype | Three iterations, ending with this version. |
| Validate | Think-aloud sessions with three testers on the first version. |

## How AI was used

Claude acted as an ideation and build partner. Each suggestion was recorded as adopted, modified or rejected.

- **Adopted:** the time-based position engine and the combined wait plus ride estimate.
- **Modified:** a larger network became three routes, and route identity moved to colour.
- **Rejected:** real GPS hardware, user accounts and multi-city support.

All generated logic was reviewed and tested before it was accepted.

## Limitations

- Positions, delays and seat levels are simulated, not live GPS.
- Buses on a route are evenly spaced, and the map is a schematic, not a real street map.
- Fares are an assumed formula, not an official tariff.
- Pinned stops and preferences stay in one browser.

## Future work

- [ ] Replace the simulation with a real GPS feed (the ETA logic stays the same)
- [ ] Push notifications for arrivals and delays
- [ ] Driver companion app for status updates
- [ ] Journeys that change routes
- [ ] Real street map and audio announcements
- [ ] Usability testing with a larger group


