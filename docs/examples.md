# Examples and exercises

Every example below is in the tool's **Load example** menu, and can also be opened directly with `ConformalMapTool["name"]`. For each one: what the problem is, what each map does, and what to look for while animating. Exercises for students follow at the end, with answers hidden.

| Example | Region | Maps | Shows |
| --- | --- | --- | --- |
| Parallel plates | strip between two open plates | `z` | the goal: conductors on straight lines |
| Coaxial cylinders | annulus | `Log`, rescale | `Log` turns circles about 0 into lines |
| Touching cylinders | crescent between two touching circles | one Möbius map | sending a point to infinity turns circles into lines |
| Confocal elliptic cylinders | between two confocal ellipses | `ArcCosh`, rescale | the inverse Joukowski map |
| Right-angle corner | quarter-plane bounded by two open plates | `Log`, rescale | opening a wedge into a strip |
| Two parallel wires | the plane outside two separate circles | Möbius, `Log`, rescale | non-nested conductors in an unbounded region |
| Two wires inside a shell | inside a shell, outside two separate wires | `z^2 - 1`, `Log`, rescale | several conductors side by side inside an outer boundary |

## Parallel plates

Two plates, `y = 0` at `V = 0` and `y = 1` at `V = 1`, drawn as long segments. The plates are open curves, so the region between them is described with the restriction `0 < y < 1`. The potential is `Im w`, and the identity map already solves the problem: both plates are horizontal lines `Im w = V`. This is the target picture every other example is trying to reach.

## Coaxial cylinders

`|z| = 1` at `V = 0` inside `|z| = 2` at `V = 1`; the potential is `Re w`.

- **`w1 = Log[z]`.** Since `Log z = ln|z| + i arg z`, the circle `|z| = r` becomes the vertical line `Re w = ln r`, and the rays from the origin become horizontal lines. The annulus unrolls into the rectangle `0 < Re w < ln 2`, `-π < Im w ≤ π`. Watch the shading tear along the negative real axis: that is the branch cut of `Log`, and it becomes the top and bottom edges of the rectangle.
- **`w2 = z/Log[2]`.** This moves the outer cylinder from `Re w = ln 2` to `Re w = 1`.
- **Result:** `V = ln(x² + y²)/(2 ln 2) = ln r/ln 2`. The equipotentials are circles and the field lines are rays.

## Touching cylinders

This is the example from version 1. A cylinder of radius 2 centred at `2i` (`V = 0`) contains a cylinder of radius 1 centred at `3i` (`V = 1`). The two touch at `z = 4i`. The potential is `Im w`.

- **`w1 = -i z/(z - 4i)`** is a Möbius map that sends the touching point `4i` to infinity. Möbius maps send circles to circles or lines, and a circle through the point sent to infinity becomes a line. Both circles pass through `4i`, so both become lines, parallel because the circles were tangent. The constants are chosen so the lines are `Im w = 0` and `Im w = 1`.
- **What to notice:** the thin part of the crescent near the touching point is stretched out to infinity, so most of the strip comes from a tiny region near `4i`. Zoom out on the right plot to see more of it.

## Confocal elliptic cylinders

The ellipse with semi-axes 5/4 and 3/4 (`V = 0`) lies inside the ellipse with semi-axes 5/3 and 4/3 (`V = 1`). Both have foci at `±1`.

- **`w1 = ArcCosh[z]`.** The Joukowski-type map `z = cosh w` sends the vertical line `Re w = u` to the ellipse with semi-axes `cosh u` and `sinh u`, so `ArcCosh` does the reverse. The inner ellipse has `cosh u = 5/4`, so `u = ln 2`. The outer has `cosh u = 5/3`, so `u = ln 3`.
- **`w2 = (z - Log[2])/Log[3/2]`** moves the lines to `Re w = 0` and `Re w = 1`.
- **What to notice:** the branch cut of `ArcCosh` along the real axis to the left of 1 tears the region, just as `Log` does for the coaxial cylinders.

## Right-angle corner

Two plates meeting at the origin: the positive x-axis at `V = 0` and the positive y-axis at `V = 1`. The region is the quarter-plane `x > 0 && y > 0`, given as a restriction because the plates are open curves. The potential is `Im w`.

- **`w1 = Log[z]`** sends the ray at angle θ to the horizontal line `Im w = θ`. The corner opens into the strip `0 < Im w < π/2`.
- **`w2 = 2 z/Pi`** rescales the strip to `0 < Im w < 1`.
- **Result:** `V = (2/π) arg z`. The equipotentials are rays from the corner and the field lines are circular arcs around it.

## Two parallel wires

Two wires of radius 1 centred at `x = -2` (`V = -1`) and `x = 2` (`V = 1`), in an otherwise empty plane. Neither conductor is inside the other, and the region extends to infinity; both wires are *conductors inside the region* and there is no outer boundary.

- **`w1 = (z - a)/(z + a)` with `a = √3`.** The points `±a` are the common inverse points of the two circles, so this Möbius map sends both circles to circles centred on 0. The two wires become *concentric*. The point at infinity goes to `w = 1`; the small hole near `w = 1` in the second plane is the far edge of the view box.
- **`w2 = Log[z]`** turns the concentric circles into vertical lines, as in the coaxial example.
- **`w3 = -z/Log[2 + Sqrt[3]]`** puts them at `Re w = -1` and `Re w = 1`.
- **What to notice:** the field lines are circles through `±√3`, the classic picture of the field of two line charges.

## Two wires inside a shell

Two wires side by side, both at `V = 0`, inside a shell at `V = 1`. Neither wire is inside the other; both sit inside the outer boundary. All three surfaces are Cassini ovals `|z² - 1| = b²`, with `b = 1/2` for the wires and `b = 2` for the shell. The ovals are entered as `Re[...]`/`Im[...]` of complex parametrisations; the shell's parameter runs from 0 to 4π because it takes two turns of `e^{it}` to go round it once.

- **`w1 = z^2 - 1`** is two-to-one: it folds the left wire and the right wire onto the *same* circle `|w| = 1/4`. The shell becomes the circle `|w| = 4`. The two-holed region becomes an annulus, covered twice.
- **`w2 = Log[z]`** and **`w3 = (z + Log[4])/Log[16]`** finish the job as in the coaxial example.
- **Why both wires must be at the same potential here:** after `w1`, both wires lie on the same circle, so any function of `w1` takes the same value on both. Wires at different potentials need a different construction.

## Exercises

These can all be done in the tool. Start from the example named in each exercise, then change the surfaces or maps.

**1. Coaxial cylinders of any size.** Change the cylinders to radii `a = 1/2` (`V = 0`) and `b = 3` (`V = 1`). Keep `w1 = Log[z]`. What should `w2` be?

<details><summary>Answer</summary>

`w2 = (z - Log[1/2])/Log[6]`. In general it is `(z - Log[a])/Log[b/a]`.

</details>

**2. A third circle.** In *Touching cylinders*, replace the inner cylinder by the circle of radius 3/2 centred at `5i/2`: `x(t) = -3/2 Sin[t]`, `y(t) = 5/2 + 3/2 Cos[t]`. It also touches the outer cylinder at `4i`. Without changing the map, what potential must it have for the check to pass?

<details><summary>Answer</summary>

`V = 1/3`. The map sends it to the line `Im w = 1/3`, so it is one of the equipotentials of the original problem.

</details>

**3. A narrower wedge.** Starting from *Right-angle corner*, make the second plate the ray at 45°: `x(t) = t/Sqrt[2]`, `y(t) = t/Sqrt[2]`. Change the region restriction to `y > 0 && y < x`. Fix the second map.

<details><summary>Answer</summary>

`w2 = 4 z/Pi`. For a wedge of angle α, use `z/α` after `Log`.

</details>

**4. A step on the boundary.** Solve the half-plane problem with `V = 1` on the negative real axis, `V = 0` on the positive real axis, and region `y > 0`. Use two open plates along the axis.

<details><summary>Answer</summary>

Use `w1 = Log[z]`, `w2 = z/Pi` with the potential `Im w`. Then `V = arg(z)/π`.

</details>

**5. A grounded cylinder in a uniform field.** Load *Blank*: a unit-circle conductor at `V = 0`, with the region outside it. Show that the single map `w = z - 1/z` with potential `Re w` satisfies the boundary condition. Look at the field lines. What does the potential look like far from the cylinder?

<details><summary>Answer</summary>

On `|z| = 1`, `z - 1/z = 2 i sin t` is purely imaginary, so `Re w = 0`. Far away, `Re w ≈ x`, a uniform field. The field lines bend around the cylinder and meet it at right angles.

</details>

**6. A bigger shell.** In *Two wires inside a shell*, replace the shell by the Cassini oval `|z² - 1| = 9`. That is `x(t) = Re[3 Exp[I t/2] Sqrt[1 + Exp[-I t]/9]]` and `y(t) = Im[...]` of the same expression, with `t` from 0 to `4 Pi`. Which map has to change, and to what?

<details><summary>Answer</summary>

Only the last one: `w3 = (z + Log[4])/Log[36]`. After `z^2 - 1` and `Log`, the shell is at `Re w = Log[9]` and the wires are at `Re w = -Log[4]`.

</details>

**7. Think about it.** In *Two wires inside a shell*, set the left wire to `V = 0` and the right wire to `V = 1/2`. Why can no choice of `w2` and `w3` fix the check? What would have to change?

<details><summary>Answer</summary>

After `w1 = z^2 - 1`, both wires lie on the same circle. Every later map acts on `w1`, so it gives the same value on both wires. The first map would have to treat the two wires differently. This is the main difficulty with multiply connected regions.

</details>
