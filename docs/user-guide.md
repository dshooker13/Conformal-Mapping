# User guide

This guide walks through every part of the tool. If you only want to try it, the [Getting started](../README.md#getting-started) section of the README is enough.

## Opening the tool

Open `ConformalMappingTool.nb`, click the cell `ConformalMapTool[]` and press <kbd>Shift</kbd>+<kbd>Enter</kbd>. Answer **Yes** if Mathematica asks to evaluate the initialization cells, and **Enable** if it asks about dynamic content.

| Command | Opens the tool with |
| --- | --- |
| `ConformalMapTool[]` | the coaxial-cylinders example |
| `ConformalMapTool["name"]` | a named example (see the **Load example** menu for the names) |
| `ConformalMapTool["Blank"]` | one unit-circle conductor and the identity map |

You can open several copies of the tool in the same notebook; each one keeps its own state.

## Layout

The controls are on the left, in four collapsible sections. The plots and results are on the right:

- **Left plot: the physical region (the z-plane).** It always shows the original problem.
- **Right plot: the current stage.** It moves through the chain of maps as you animate.
- Under the plots: the animation controls, the **boundary-condition table**, the **solution panel** and any **warnings**.

Every change you make (typing in a field and pressing <kbd>Enter</kbd>, choosing from a menu, moving a slider) triggers a recomputation. While it runs, the plots keep showing the previous result. Recomputation usually takes about a second; it takes longer at fine resolution or once the equipotentials are being drawn.

## Load example

The **Load example** menu replaces the current problem with a worked example. Its title and a short description appear under the menu. See [examples.md](examples.md) for what each one demonstrates.

## 1. Conductors and boundary curves

### The surface table

Each row is one surface:

| Column | Meaning |
| --- | --- |
| ◉ | Select this surface for editing |
| colour | The colour used for this surface in every plot |
| Name | Your label for it |
| Role | How it relates to the region (see below) |
| Potential | The required potential `V` |
| Check | ✓ satisfied, × not satisfied, — not checked, ! input error, ? undetermined |

**Add surface** appends a unit circle. **Remove selected** deletes the selected surface (the last one cannot be removed).

### Editing the selected surface

| Field | What to type |
| --- | --- |
| Name | Any text |
| `x(t)`, `y(t)` | The curve, as expressions in `t`, for example `2 + Cos[t]` and `Sin[t]` |
| `t` from … to | The parameter range, for example `0` and `2 Pi` |
| Potential `V` | A number, or an expression in `t` if the potential varies along the surface. Untick **known** if you don't know it (see below) |
| Role | See below |

Press <kbd>Enter</kbd> (or click elsewhere) to apply a change.

**Only `t` may appear** in `x(t)`, `y(t)` and `V`. Constants must be numbers: write `3/2`, not `a`. A misspelt function such as `cos[t]` is reported as an undefined symbol.

### Roles: describing the region

The region where the potential is wanted is:

- **inside** the surface marked *outer boundary of the region* (if there is one),
- **outside** every surface marked *conductor inside the region*,
- satisfying the **region restriction** (section 3), if you gave one,
- and inside the view box.

The three roles are:

| Role | Use it for | Example |
| --- | --- | --- |
| conductor inside the region | a closed conductor surrounded by the region; the region is outside it | the inner cylinder of a coaxial cable; each of two parallel wires |
| outer boundary of the region | a closed surface enclosing the region; the region is inside it | the outer cylinder of a coaxial cable; a grounded shell |
| open curve (encloses nothing) | plates, sheets, or any boundary that doesn't close up | the plates of a capacitor; the two sides of a corner |

Any number of conductors can sit inside the region, side by side. They don't have to be nested inside one another. There is at most one outer boundary. With no outer boundary, the region extends to infinity and is drawn truncated to the view box.

A curve counts as **closed** when its two ends meet: `(x(t), y(t))` at the start of the `t` range equals the point at the end, to within 10⁻⁶. A surface marked as a conductor or outer boundary that is not closed is treated as an open curve, and a warning says so.

### Surfaces with unknown potential

Untick **known** next to the potential to leave a surface out of the check. It is still drawn and carried through the maps. The check table then reports where it landed. If it lies on a single line, the table says which one ("not checked; lies on Im w = 0.3333"), which is that surface's potential in the solution. This is useful for probing an equipotential, or for watching any curve transform. At least one surface must have a known potential for the problem to count as solved.

### Following a point

Tick **follow a point along the selected surface** and move the slider (0 is the start of the `t` range, 1 the end; the slider can also animate). A marker shows the point in both plots, moving with the animation, and a readout under the animation controls lists its position in every plane: `z ↦ w1 ↦ w2 ↦ …`. A point sent to infinity is shown as ∞.

### Tips for describing curves

| Curve | `x(t)` | `y(t)` | `t` range |
| --- | --- | --- | --- |
| circle, centre (a, b), radius r | `a + r Cos[t]` | `b + r Sin[t]` | `0` to `2 Pi` |
| ellipse, semi-axes p, q | `p Cos[t]` | `q Sin[t]` | `0` to `2 Pi` |
| segment from (x1, y1) to (x2, y2) | `x1 + t (x2 - x1)` | `y1 + t (y2 - y1)` | `0` to `1` |
| an "infinite" line y = c | `t` | `c` | e.g. `-5` to `5` |
| any curve z(t) given as a complex expression | `Re[z(t)]` | `Im[z(t)]` | as needed |

- Infinite plates are represented by long segments. Make them at least as long as the region you want to see.
- The `Re[...]`/`Im[...]` form is handy for curves defined by a map, such as the Cassini ovals in the *Two wires inside a shell* example.
- The direction in which a curve is traced doesn't matter.

## 2. Conformal maps

Each map `wk(z)` is an expression in `z`. The maps are applied in order: `w1` acts on the z-plane, `w2` acts on the output of `w1`, and so on, so the composite map is `W(z) = wn( … w2(w1(z)))`. In every field the variable is called `z`, even though it stands for the previous plane's coordinate.

- **Add map** appends the identity map `z`, ready for editing.
- **Add a common map** appends a ready-made map: shift, rotate and scale, inversion `1/z`, a Möbius map, `Log`, `Exp`, `z^2`, `Sqrt`, the Joukowski map `(z + 1/z)/2` or its inverse `ArcCosh`.
- **Remove** deletes that map.

With no maps at all, `W(z) = z`.

## 3. Boundary condition and region

- **Conductors become lines of: constant Re w / constant Im w.** This chooses which part of `W` is the potential. With *Re w*, each conductor must land on a vertical line `Re w = V`. With *Im w*, it must land on a horizontal line `Im w = V`.
- **Region restriction.** An optional condition in `x` and `y` that every point of the region must satisfy, such as `y > 0`, `0 < y < 1` or `x > 0 && y > 0`. Use `True` for none. This is how regions bounded by open curves (half-planes, strips, wedges) are described.
- **View size (regions reaching infinity).** When there is no outer boundary, the region is drawn inside a square view box. Its half-width is this many times half the size of the surfaces. Larger values show more of the far region; smaller values give more detail near the conductors.
- **Exact (symbolic) check.** Simplifies `Re W` (or `Im W`) along each surface symbolically, assuming `t` is real and within its range, and compares the result with `V`. The result appears under each row of the check table. This can take a few seconds per surface, and it gives up on very complicated expressions.

## 4. Display

| Setting | Effect |
| --- | --- |
| Show: region | Shade the region (light blue) |
| Show: checkerboard | With region shading on, use a checkerboard instead, which makes the preservation of angles easy to see |
| Show: grid | Before the problem is solved, draw a grid of horizontal and vertical lines from the z-plane, carried through the maps. Once it is solved, the grid *is* the equipotentials and field lines |
| Show: equipotentials, field lines | Draw the solution's level curves, once they exist |
| Equipotentials, Field lines | How many of each to draw: type a whole number from 0 to 50 and press <kbd>Enter</kbd> |
| Grid lines (each direction) | How many z-plane grid lines in each direction, from 0 to 60 (used before the problem is solved) |
| Shading resolution | Coarse, medium or fine shading (fine is slower) |
| refine automatically near poles and edges | Subdivide curve segments and shading cells whose images are stretched in some plane (on by default; turn it off for speed) |
| Zoom (right plot) | Zoom the right-hand plot in or out by powers of 2 |
| draw level curves even before the check passes | Show level curves of `Re W` and `Im W` even if some boundary condition fails. Useful for seeing *how* a wrong map is wrong. |

## The plots and the animation

The **stage** runs from 0 (the z-plane) to n (the final plane, after all n maps). At a whole number k, the right plot shows the region after `w1, …, wk`. Between whole numbers, it shows a straight-line blend, point by point, of the two neighbouring planes. This is simply a way to animate the step; the intermediate pictures are not themselves conformal images.

- **Animate the maps:** play, pause and drag through the whole chain.
- **Jump to:** go straight to the z-plane or the plane after any map.
- The plot window follows the region automatically. It ignores the few percent of points that were thrown very far away (near a pole of a map, for instance). Use **Zoom** if you want a closer look.
- **Lines break where they jump.** If two neighbouring points of a curve land far apart (more than 15% of the window), the curve is broken there instead of being joined across the plot. This happens across a branch cut, as with `Log` along the negative real axis, or near a pole. Shading cells that would be stretched across the plot are dropped for the same reason.
- Points sent to infinity, or to values larger than 10⁸, are dropped.
- The plots use equal scales on both axes, so angles in the pictures are true angles.

## The boundary-condition table

For each surface, the table shows:

- **Required:** the condition, `Re w = V` or `Im w = V`.
- **Actual:** the range of `Re w` (or `Im w`) over the surface's image in the final plane. For a satisfied condition with constant `V`, the two ends of the range are equal.
- **Status:** *satisfied*; *off by up to* r (the largest deviation from `V`); *not checked* (potential marked unknown; says which line it lies on, if any); *input error* (see the warnings); or *undetermined* (more than half of the surface was sent to infinity, so there is too little left to check).

A surface passes when the deviation is below 10⁻⁶ × (1 + the largest |V| on it). Points of a surface that are sent to infinity are skipped, which is normal when a Möbius map sends a point of a circle to infinity to turn the circle into a line.

After **Exact (symbolic) check**, each row also shows either *exact: holds identically* or what the condition *simplifies to*.

## The solution panel

When every surface passes, the panel shows:

- the composite map `W(z)`,
- the potential `V(x, y)` (the real or imaginary part of `W(x + i y)`, simplified),
- the colour scale of the equipotentials.

The shading becomes a mesh of curvilinear squares bounded by the equipotentials and field lines: the tool builds a rectangular grid in the final plane and pulls it back through a numerical inverse of the maps (see [how it works](how-it-works.md#7-the-solution-mesh-equipotentials-field-lines-and-shading)). With the checkerboard on, each square between neighbouring lines gets its own colour.

The equipotentials are spaced evenly between the lowest and highest conductor potentials. The range is widened if the potential inside the view box goes beyond them, as it does in some regions that extend to infinity. The field lines are level curves of the other part of `W`. In the final plane, both families are straight lines.

## Warnings

Warnings appear in orange under the solution panel. They include:

- a surface uses an undefined symbol, has an invalid `t` range, or does not give real numbers for `x(t)`, `y(t)` or `V`;
- a surface marked as a conductor or outer boundary is not closed (it is treated as an open curve);
- more than one outer boundary (only the first one is used);
- a conductor lies outside the outer boundary, or one conductor lies inside another;
- the region is empty;
- a map uses a symbol other than `z`, or sends the whole region to infinity;
- the region restriction is not a true/false condition.

## Saving and sharing

Save the notebook normally. The tool's configuration (surfaces, maps, settings) is saved with it. The computed results are not saved, which keeps the file small; they are recomputed when the notebook is reopened and dynamic content is enabled. The definitions the tool needs are stored with its output, so a saved tool keeps working in a fresh Mathematica session. If it ever doesn't, evaluate the launch cell again.

## Troubleshooting

| Symptom | Likely cause |
| --- | --- |
| A row says *input error* | Check the warnings: usually an undefined symbol, such as a constant written as a letter or a misspelt function name |
| Everything vanishes after some map | That map sends the region to infinity or produces non-numbers, for example from a symbol other than `z` |
| The shaded region isn't the one you meant | Check the roles in the surface table and the region restriction; read the warnings |
| A condition fails by a tiny amount | Usually a slightly wrong constant in a map; try the exact check |
| A condition fails near one point only | A branch cut or pole may cross that surface; try a different branch (for example, rotate first with `-z` or `I z`) |
| "Computing…" for a long time | Fine resolution, many level curves, or a complicated composite map; reduce the settings in section 4 |
