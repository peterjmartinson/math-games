# GiantTemple ("Temple Automaton Arm") Proof-of-Concept Plan

## 1. Overview & Goal
Build a zero-dependency, single-file (`index.html`) HTML5 Canvas prototype in `GiantTemple/` that tests the core mechanics of **The Giant Temple Automaton**: piloting a two-jointed ancient stone guardian crane via shoulder ($\alpha$) and elbow ($\beta$) linkages using angle-addition forward kinematics:
$$X = L_1 \cos(\alpha) + L_2 \cos(\alpha + \beta)$$
$$Y = L_1 \sin(\alpha) + L_2 \sin(\alpha + \beta)$$

The PoC captures:
1. **The Motions**: Two-segment stone automaton arm ($L_1 = 160\text{px}$, $L_2 = 140\text{px}$) anchored at `(canvas.width / 2, canvas.height - 80)`, with animated stone gears, glowing rune joints, claw pincers, and stone-rumble screen shake on relic capture.
2. **The Controls**:
   - Dual interactive **Rotary Stone Dials + Horizontal Lever Drag-Bars** for **Shoulder Angle $\alpha \in [0, \pi]$** and **Elbow Angle $\beta \in [-\pi, \pi]$** (plus keyboard `W/S`/`A/D` or arrow keys for simultaneous two-hand puppeteering!).
   - Quick-action buttons (`Fold In`, `Full Extend`, `Flip Elbow ±β`) to let players feel how $\beta = 0$ maximizes reach ($L_1 + L_2 = 300\text{px}$) while $\beta = \pm\pi$ folds the arm inward ($|L_1 - L_2| = 20\text{px}$).
3. **The Object of the Game**:
   - Position the stone claw within `15px` of a floating glowing crystal relic $(X_t, Y_t)$ and hold for `1.0s` to retrieve it, triggering a stone-rumble screen shake and advancing the **Relics Retrieved** counter.
   - Includes curated challenges inspired by `concept.txt` (extending to high vault ceilings, folding the elbow around overhanging stone pillars, and catching drifting relics) alongside endless reachable spawns.
   - Includes the **"Show Reach Geometry"** toggle that draws the component right triangles for $\alpha$ ($L_1\cos\alpha, L_1\sin\alpha$) and $(\alpha + \beta)$ ($L_2\cos(\alpha+\beta), L_2\sin(\alpha+\beta)$), plus the combined reach vector, in glowing chalk.

---

## 2. File Structure
All code lives inside `C:\Users\Admin\Documents\math-games\GiantTemple\`:
- `PLAN.md` — This implementation plan.
- `index.html` — Single-file, zero-dependency HTML/CSS/JS prototype.

---

## 3. Visual & Technical Specifications
- **Aesthetic**: Dark Mayan/Indiana Jones stone vault (`#12100e` to `#1c1712`), carved stone border reliefs, flickering torchlight radial glow, and carved stone segment beams with turquoise/amber rune inlays.
- **Forward Kinematics Engine**:
  - Anchor `(Ax, Ay) = (canvas.width / 2, canvas.height - 80)`.
  - Elbow `(Ex, Ey) = (Ax + L1 * cos(α), Ay - L1 * sin(α))`.
  - Claw `(Hx, Hy) = (Ex + L2 * cos(α + β), Ey - L2 * sin(α + β))`.
- **Glowing Chalk Geometry Overlay (`Show Reach Geometry`)**:
  - Right triangle 1 (amber chalk): Base $L_1\cos\alpha$, Height $L_1\sin\alpha$, arc for $\alpha$.
  - Right triangle 2 (cyan chalk): Base $L_2\cos(\alpha+\beta)$, Height $L_2\sin(\alpha+\beta)$, arc for $\alpha+\beta$ relative to horizontal and $\beta$ relative to $L_1$.
  - Combined resultant vector dashed from Shoulder Anchor to Claw `(X, Y)`.
- **Relic Capture & Hazards**:
  - Distance check `hypot(Hx - Xt, Hy - Yt) <= 15px` fills a 1.0-second circular rune charge ring around the relic.
  - Stone obstacle blocks on later relics require folding the arm (using negative/positive $\beta$ configurations) so the elbow doesn't collide with vault pillars.
