# Traffic Microsim

[![100% AI-generated](https://img.shields.io/badge/100%25-AI--generated-8b5cf6?logo=claude&logoColor=white)](#entirely-ai-generated)
[![Live demo](https://img.shields.io/badge/▶-live_demo-4ade80)](https://saintpepsi.github.io/traffic-microsim/)
[![License: MIT](https://img.shields.io/badge/license-MIT-7aa2ff)](LICENSE)

A single-file traffic microsimulation you can run by double-clicking — no build, no dependencies. Watch cars with distinct personalities follow, overtake, jam, and obey (or flout) the speed limit on a vertical south→north road.

**▶ Live demo: https://saintpepsi.github.io/traffic-microsim/**

![Traffic microsim demo](demo.gif)

## What it does

Each car makes two decisions every tick — how fast to go, and whether to change lanes — using two classic, well-known models:

- **IDM (Intelligent Driver Model)** for car-following: a car accelerates toward its desired speed and brakes for the car ahead, keeping a *time-based* gap (the 2-second rule). [Reference](https://en.wikipedia.org/wiki/Intelligent_driver_model)
- **MOBIL** for lane changes: a car switches lane if it gains acceleration (and a keep-side bias nudges it), as long as it won't force the new follower to brake too hard. [Reference](https://traffic-simulation.de/info/info_MOBIL.html)

All driving personality is **data** — there are no `if (type === 'overtaker')` branches anywhere. Four profiles fall out of the parameters:

| Profile | Behaviour |
|---|---|
| 🟢 **Hugger** | Sits in the slow lane, rarely moves |
| 🟡 **Overtaker** | Weaves through traffic to get ahead |
| 🔵 **Cautious** | Slower, big following gaps |
| 🔴 **Aggressive** | Tailgates, accelerates hard |

## Controls

**Driver behaviour (left)** — edit any type's behaviour live (no restart): spawn %, speed vs the limit, following distance, acceleration, lane-change eagerness, politeness, keep-side bias. Hover any **?** for an explanation.

**Simulation (right)** — sim speed (0.25×–4×, 1× = real time), speed limit (soft — drivers scatter around it), car count, lane count, pause/step, drive-on-left/right, and a live list of cars in view you can click to follow.

## How it's built

The simulation core is pure functions (`idmAccel`, `mobilDecision`, `stepLongitudinal`, `stepLateral`) with no DOM access, so it's deterministic and testable; a separate `render()` reads state and draws to a canvas. The road is a closed 3 km ring rendered as an endless straight road via a follow-camera. A handful of `console.assert` invariants run on load (open DevTools to see them pass).

It's intentionally "naive": one straight multi-lane road, no junctions, on-ramps, or weather — just longitudinal following and lateral lane-changing done well enough to look real and produce emergent jams.

## Run locally

```bash
git clone https://github.com/SaintPepsi/traffic-microsim.git
open traffic-microsim/index.html      # or just double-click it
```

## Entirely AI-generated

This project — the simulation code, the UI design, this README, even the demo GIF — was built **entirely by an AI agent** (Claude, via Claude Code) through a conversation: brainstorming the IDM/MOBIL approach, implementing it test-first, then iterating on the visuals and controls. No line was hand-written by a human. The commit history is the full record.

## License

MIT — see [LICENSE](LICENSE).
