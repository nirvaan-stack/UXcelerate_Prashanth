# 🛰️ SOTERIA — Post-Earthquake Multi-Robot Coordination Interface

> **UXcelerate! 2026 Submission** — Design for Impact.
> An interface for coordinating rescue robots after an earthquake: incomplete maps, unreliable
> communication, and continuous discoveries of survivors, blocked paths, structural hazards and new routes.

---

## 🎯 The Problem, Decomposed

| Challenge in the brief | How SOTERIA answers it |
|---|---|
| **Maps may be incomplete** | Fog-of-war map: only robot-explored sectors are visible. Unexplored areas are explicitly rendered as *unknown*, never as "empty". Coverage % is a first-class mission stat. |
| **Communication may be unreliable** | One-click "Simulate Comms Degradation" drops the link: robots hold on stale orders, signal indicators go red, and every command enters a **store-and-forward queue** that replays automatically on restore. The UI never blocks the operator — it always stays responsive with optimistic, clearly-labeled states. |
| **Robots continuously discover things** | A live event stream: survivors (3 triage levels), structural hazards/collapse zones (which *dynamically block* previously-open cells), gas leaks, and newly accessible routes (cells re-open). Each discovery triggers a prioritized triage queue re-sort. |

---

## 🧠 Core UX Decisions (what to say to judges)

1. **Triage, not just tracking.** Survivors are auto-scored `P = severity_weight − age_decay − hazard_proximity_penalty`.
   Critical ≠ highest priority forever — a stable survivor sitting next to a collapsing structure outranks an aging urgent case. This models real FEMA/INSARAG triage logic.

2. **Path-aware dispatch, not Euclidean.** "Dispatch nearest robot" computes **BFS over the actual rubble grid** — a robot 20m away behind a collapsed wall correctly loses to one 80m away with a clear route. This is the single detail that makes the interface believable.

3. **Design for degraded comms, not against it.** When the link drops, the operator's workflow *does not change* — commands queue, state is labelled "SIGNAL LOST — HOLDING", and everything replays on restore. Predictable > perfect.

4. **Never color alone.** Critical = solid ring, Urgent = dashed ring, Stable = dotted ring; hazards use an icon (⚠), robots use triangles. WCAG-minded: keyboard navigable panels (`Tab`/`Enter`), `aria-label`s on the map and live toasts region, focus outlines, blinking indicators kept subtle.

5. **3-pane ops-room layout** (industry standard for dispatch consoles): **Fleet** (left) → **Map** (center) → **Triage & tasking** (right). Muscle-memory placement reduces error under stress.

---

## ▶️ Run it

Open `index.html` in any browser — zero dependencies, zero build step. It works offline (fitting, no?).

**Try this 60-second demo script:**
1. Let it run — watch robots explore the fog, survivors appear in the triage queue.
2. Hit **"Simulate Comms Degradation"** → dispatch a survivor → command queues. Disable degradation → watch it replay.
3. Toggle the **heatmap layer** to see survivor-density clustering.
4. Note hazards re-blocking the map and "new accessible route" events opening it back up.

**Shortcuts:** `Space` = pause · `D` = toggle comms · click any survivor/robot to inspect.

---

## 🏗️ Architecture

- Vanilla HTML/CSS/JS, single file, canvas-rendered tactical map.
- Grid world (48×48) with procedural rubble; robots run BFS pathfinding.
- Deterministic triage scoring; event log + toasts (`aria-live`) for situational awareness.
- Store-and-forward command queue decoupled from the simulation loop.

## 🔮 Next steps (roadmap slide material)

- Mesh-network topology view (robots as relay nodes when infrastructure is down)
- Offline-first PWA with Service Worker + CRDT sync for multi-operator teams
- Voice command layer for hands-busy operation
- Digital-twin replay mode for after-action review

---

*Built with 🖤 for the people who run toward the rubble.*
