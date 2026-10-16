Generate a single-file HTML/JS/CSS interactive prototype called "Temple Automaton Arm" set inside a Mayan/Indiana Jones-style stone vault.

REQUIREMENTS & ARCHITECTURE:
1. Zero external libraries. Pure vanilla JavaScript and HTML5 Canvas.
2. Visual Aesthetic:
   - Dark cave background with carved stone textures and torchlit ambiance.
   - The automaton is a two-joint stone crane anchored at (canvas.width / 2, canvas.height - 80).
   - Base Segment (Shoulder): length L1 = 160px, angle α.
   - Forearm Segment (Elbow): length L2 = 140px, angle β relative to the base segment.
   - Hand/Claw: position (X, Y) determined by forward kinematics:
       X = L1 * cos(α) + L2 * cos(α + β)
       Y = L1 * sin(α) + L2 * sin(α + β)
3. Controls:
   - Two large, responsive onscreen rotary stone knobs or dual horizontal drag-bars:
     * Knob 1: Shoulder Angle α (sweeps 0 to π).
     * Knob 2: Elbow Angle β (sweeps -π to π).
   - Real-time kinematic lines drawn cleanly showing the individual angle components and the combined reach vector.
4. Game Objective:
   - Spawn a floating glowing relic / crystal orb at a target coordinate (Xt, Yt).
   - The player must adjust α and β to position the claw within 15px of the relic and hold it there for 1 second.
   - Once collected, trigger a stone-rumble screen shake effect, increment a "Relics Retrieved" counter, and spawn the next target in a valid reachable coordinate.
   - Include a toggle: "Show Reach Geometry", which visualizes the component right triangles formed by α and (α + β) in faint glowing chalk.