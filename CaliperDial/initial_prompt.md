Generate a single-file, zero-dependency HTML/JS/CSS prototype to test the core mechanics of a puzzle game based on trigonometric projections and gear multipliers.

[GAME CONCEPT CONTEXT]
The final game is an archaeological puzzle adventure where a player manipulates ancient astrolabes and sliding stone calipers to unseal vault doors. Rather than doing symbolic algebra, players manipulate dials that cast shadow bars along orthogonal axes (representing sine and cosine). Solving puzzles requires discovering that different angle configurations and gear pairings (such as double-angle couplings) produce the exact reaches required to trip multi-point lock pins.

[PROOF-OF-CONCEPT SPECIFICATIONS]
Create an ultra-minimal, high-contrast 2D wireframe on HTML5 Canvas. No textures, no themes, no skeuomorphic art—only geometric lines, circles, and coordinate readouts.

1. Layout & Visual Elements:
   - Clean dark background (#111111) with white/gray line art.
   - Left side: A clean unit circle (radius R = 160px) centered on its own coordinate axes.
   - A single rotating radius vector (arm) with an endpoint handle that the user can click/drag around the circle.
   - Right wall: A vertical axis line showing a sliding indicator mark tracking the vertical projection (sin θ).
   - Bottom floor: A horizontal axis line showing a sliding indicator mark tracking the horizontal projection (cos θ).
   - Draw thin, dashed projection lines connecting the arm's tip to both axis marks.

2. Modes:
   - Add a toggle button: "Direct Mode (1x)" vs "Coupled Mode (2x)".
   - In Direct Mode, the sliders track cos(θ) and sin(θ).
   - In Coupled Mode, draw a secondary dashed arm rotating at 2θ, and the sliders track cos(2θ) and sin(2θ).

3. Game Loop & Win Condition:
   - Place a target tick mark on the vertical axis (e.g., at sin = 0.87) and another on the horizontal axis (e.g., at cos = 0.50).
   - Display a live status indicator: "LOCKED" (red text) or "ALIGNED" (green text).
   - When both slider marks sit within ±0.03 of the target ticks, snap status to "ALIGNED" and turn the lines green.
   - Include a "Next Puzzle" button that assigns new valid coordinate targets.

4. UI / Data Readout:
   - Display live numbers: Angle θ (in both radians, e.g. "0.33π", and degrees), current X projection, and current Y projection.