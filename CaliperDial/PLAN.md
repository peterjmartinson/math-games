# CaliperDial Proof-of-Concept (PoC) Plan

## 1. Overview & Goal
Build a zero-dependency, single-file (`index.html`) HTML5 Canvas prototype in `CaliperDial/` that tests the core mechanics of **CaliperDial**: manipulating an astrolabe dial to cast orthogonal projections ($\cos\theta$ and $\sin\theta$) onto sliding caliper tracks to trip multi-point lock pins.

The PoC stays ultra-minimal (high-contrast 2D wireframe) while capturing three essential pillars:
1. **The Motions**: Smooth rotary dragging on a unit circle, coupled $2\theta$ gear motion, and real-time orthogonal projection lines driving horizontal and vertical caliper sliders.
2. **The Controls**: Direct mouse/touch dragging of the primary dial handle, optional angle-invert ($\theta \to -\theta$) and mode toggles (`1x Direct` vs. `2x Coupled`), plus subtle snapping near cardinal/target angles.
3. **The Object of the Game**: Solving a sequence of curated "Vault Lock" puzzles—including obstructed-arc puzzles where direct angles are blocked (forcing the player to use negative-angle symmetries or $2\theta$ double-angle gear couplings to reach target lock pins).

---

## 2. File Structure
All code will live inside `C:\Users\Admin\Documents\math-games\CaliperDial\`:
- `PLAN.md` — This implementation plan.
- `index.html` — Single-file, zero-dependency HTML/CSS/JS prototype.

---

## 3. Visual Layout & Wireframe Design (`#111111` Canvas)
- **Canvas Geometry (800×600px logical coordinate space)**:
  - **Unit Circle (Astrolabe Dial)**: Centered at `(300, 260)` with radius `R = 160px`. Includes subtle degree/radian tick marks and coordinate axes.
  - **Primary Arm ($\theta$)**: Solid white radius vector with a draggable circular handle at $(R\cos\theta, R\sin\theta)$.
  - **Coupled Arm ($2\theta$, active in 2x Mode)**: Dashed cyan/amber radius vector rotating at twice the primary angle, casting the active projections when coupled.
  - **Blocked Sector / Obstruction (Puzzle Mechanic)**: Red hatched arc segment on the unit circle for levels where direct angles are physically blocked, forcing the player to route around or use $2\theta$ gearing.
  - **Right Wall (Vertical $\sin$ Caliper Track)**: Vertical line at `x = 560`, spanning `[260 - R, 260 + R]`. Features target lock notch(es) and a sliding caliper bar tracking the active vertical projection.
  - **Bottom Floor (Horizontal $\cos$ Caliper Track)**: Horizontal line at `y = 500`, spanning `[300 - R, 300 + R]`. Features target lock notch(es) and a sliding caliper bar tracking the active horizontal projection.
  - **Projection Beams**: Thin dashed lines connecting the active arm tip to both the horizontal ($\cos$) and vertical ($\sin$) caliper sliders. Turn bright green (`#4ade80`) when aligned within tolerance.

---

## 4. Controls & Interactive Mechanics
1. **Dial Dragging**:
   - Click and drag anywhere near the dial handle (or along the circle rim) to rotate $\theta$ via `Math.atan2`.
   - Clamp or block movement into red obstructed sectors so players physically feel the puzzle constraint.
2. **Gear Mode Toggle (`Direct 1x` vs. `Coupled 2x`)**:
   - **Direct Mode (`1x`)**: Calipers track $(\cos\theta, \sin\theta)$.
   - **Coupled Mode (`2x`)**: Secondary arm activates at $2\theta$; calipers track $(\cos 2\theta, \sin 2\theta)$.
3. **Quick-Flip Button (`Invert -\theta`)**:
   - Mirrors the dial across the horizontal axis ($\theta \to -\theta$), demonstrating $\cos(-\theta) = \cos\theta$ and $\sin(-\theta) = -\sin\theta$ from `concept.txt`.
4. **Live Telemetry & HUD**:
   - Displays $\theta$ in degrees and radians (e.g., `0.33π rad / 60.0°`), active effective angle ($1\theta$ or $2\theta$), live `X (cos)` and `Y (sin)` readouts vs. target coordinates, and lock state (`LOCKED` in red vs. `ALIGNED` in green).

---

## 5. Game Object: Curated Puzzle Progression + Free Generator
To capture the actual *object of the game* rather than just random numbers, `Next Puzzle` cycles through 4 short, designed stages before entering endless random challenges:
1. **Stage 1 — Calibration (Direct 1x)**: Simple target at $(\cos = 0.50, \sin = 0.87)$ ($\theta = \pi/3$). Learn how dragging the dial moves both orthogonal sliders simultaneously.
2. **Stage 2 — The Trench Reflection (Negative Angle Identity)**: Target requires $\cos = 0.71$, but the upper quadrant is blocked by a stone obstruction—player must sweep into the negative angle ($\theta = -\pi/4$) or use the invert control to hit $(\cos = 0.71, \sin = -0.71)$.
3. **Stage 3 — Double-Gear Reach (Coupled 2x)**: An obstruction blocks the target quadrant ($[\pi/2, \pi]$) on the primary dial. Player must engage **Coupled Mode (2x)** and place the primary dial at $\theta = \pi/3$ so the $2\theta$ arm reaches $2\pi/3$ ($\cos = -0.50, \sin = 0.87$) unhindered.
4. **Stage 4+ — Endless Vault Locks**: Procedurally generated valid targets (with optional random arc obstructions) solvable in either `1x` or `2x` mode.

---

## 6. Implementation Steps
1. **Scaffold `CaliperDial/index.html`**: Set up the responsive dark wireframe layout, HUD panel, control buttons, and `<canvas>`.
2. **Implement Math & State Engine**: Store `theta`, `mode` (`1` or `2`), `puzzleIndex`, target `(targetCos, targetSin)`, optional `blockedArc`, and alignment check (`|cos - targetCos| <= 0.03 && |sin - targetSin| <= 0.03`).
3. **Build Canvas Renderer**: Draw grid/axes, unit circle, obstructed arc sector, primary $\theta$ arm, secondary $2\theta$ arm, dashed projection lines, and right/bottom caliper tracks with target lock pins.
4. **Wire Pointer Input & Puzzle Loop**: Add mouse/touch drag handlers with arc-obstruction collision, mode/invert toggles, and stage progression via the `Next Puzzle` button.
