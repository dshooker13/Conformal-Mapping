# How it works

## The problem

In a two-dimensional electrostatics problem (conductors that are long cylinders of arbitrary cross-section, or plates), the potential `V(x, y)` in the charge-free region between the conductors satisfies Laplace's equation

  ∂²V/∂x² + ∂²V/∂y² = 0,

and takes a prescribed value on each conductor. Given those boundary values (and suitable behaviour at infinity if the region is unbounded), the solution is unique.

## Why conformal maps help

Write `z = x + i y`. If `W(z) = Φ(x, y) + i Ψ(x, y)` is analytic in the region, the Cauchy–Riemann equations make both `Φ` and `Ψ` harmonic: each one satisfies Laplace's equation. Three facts turn this into a method:

1. **Compositions of analytic maps are analytic.** You can build `W` one step at a time, `W = wn ∘ … ∘ w1`, each step a map you understand: a Möbius transformation, `Log`, a power, and so on.
2. **Straight lines are easy.** In the final w-plane, if every conductor lies on a vertical line `Re w = Vk`, then the function `Re w` takes exactly the right value on every conductor.
3. **Uniqueness.** Pulled back to the z-plane, `Φ(x, y) = Re W(x + i y)` is harmonic in the region and has the right boundary values, so it *is* the potential.

The level curves of `Ψ = Im W` cross the equipotentials at right angles. They are the **field lines**. The electric field itself is `E_x + i E_y = -conj(W'(z))`. If you choose *Im w* as the potential instead, the roles of `Re` and `Im` swap, and the conductors must land on horizontal lines.

Nothing here requires the conductors to be nested. Any region works, with any number of conductors side by side, as long as `W` is analytic in the region and `Re W` is single-valued there (the conjugate `Im W` may jump across a branch cut, as with `Log`). What is hard is *finding* such a `W`. For most shapes no closed-form `W` is known, which is why the method is usually taught on the classic geometries in the examples.

## What the tool computes

The tool does not search for maps. It takes the maps you give it and computes everything numerically, which is what lets it handle any geometry. Here are the steps, with the default settings in parentheses.

### 1. Sampling the surfaces

Each surface `(x(t), y(t))` is sampled at evenly spaced values of `t` over its range (400 points). The expressions are typed into input fields and kept unevaluated. Every symbol named `t` (or `z` in a map, or `x`, `y` in the region restriction) is replaced by an internal formal symbol before evaluation, so a value assigned to `t` somewhere else in the notebook can't interfere. Any other leftover symbol is reported as undefined.

A surface is **closed** when its last sample coincides with its first, to within 10⁻⁶ relative to its size.

### 2. Deciding what is in the region

A point is in the region when it is:

- inside the view box,
- inside the outer boundary, if there is one,
- outside every conductor marked as inside the region,
- and satisfies the region restriction.

"Inside a closed surface" uses the even–odd rule on its sampled polygon. Cast a ray to the right from the point and count how many edges of the polygon it crosses; an odd count means inside. Each test involves only the point and one curve, so the conductors can be arranged in any way: side by side, inside an outer boundary, or with no outer boundary at all.

The **view box** is square. With an outer boundary, it is that boundary's bounding square, slightly enlarged. Without one, it is centred on the surfaces and made larger by the *View size* factor (2 by default), so that a region extending to infinity is shown truncated.

### 3. What gets drawn

Three sets of points are set up in the z-plane:

- **cells:** the cells of a 61 × 61 grid over the view box whose four corners are all in the region; they are drawn as the shaded region;
- **grid lines:** 12 horizontal and 12 vertical lines across the box, 300 points each, keeping only the points in the region;
- **boundaries:** the sampled surfaces.

Once the boundary conditions are satisfied, two more sets are added (see step 7): the equipotentials and the field lines.

**Adaptive refinement.** Even sampling is too coarse wherever a map stretches things, above all near a pole. After a first pass that fixes the size of every plane's window, the tool refines:

- A **curve segment** is halved when, in any plane, its image is longer than 1% of that plane's window, or when one end can be drawn and the other cannot (it lies at a pole or outside the region). This is repeated up to 6 times for surfaces and 4 times for grid lines, within a budget of 3–4 times the original number of points.
- A **shading cell** is split into four when it straddles the edge of the region, or when its image in some plane is larger than 3% of the window. This is repeated twice, within a budget of 3 times the original number of cells.

This is the automatic version of the "plot performance tuning" that version 1 left to the user.

### 4. Pushing points through the maps

Each map is applied numerically to every point of the previous plane, using Mathematica's principal branches of `Log`, `Sqrt`, `ArcCosh` and so on. A point that becomes infinite, undefined, non-numeric, or larger than 10⁸ is dropped from that plane and all later ones. The tool keeps every intermediate plane.

This is the main design difference from version 1, which needed the inverse of every map to draw the solution. Everything here is mapped **forward**, so no inverses, branch choices or root-finding are needed.

### 5. Animating

The stage `s` runs from 0 to n. Between whole numbers k and k + 1, each point is drawn at `(1 - u) zk + u zk+1`, where `u = s - k`. The plot window is interpolated the same way, so the view glides from one plane to the next. Each plane's window is chosen to contain the region, ignoring the most extreme 3% of points in each direction; otherwise one point near a pole would shrink everything else to a dot.

A curve is broken wherever a point was dropped or two neighbouring points are more than 15% of the window apart. A shading cell is dropped for the same reasons. This is what makes branch cuts show up as clean tears, rather than as lines drawn across the plot.

### 6. Checking the boundary conditions

In the final plane, the tool compares `Re w` (or `Im w`) at every surviving sample of each surface with the required potential at that sample. A surface passes when the largest difference is below 10⁻⁶ × (1 + max |V|). Samples sent to infinity are skipped; if more than half are lost, the result is "undetermined".

The **exact check** works symbolically instead. It substitutes the surface's parametrisation into the composite map, takes the real (or imaginary) part with `ComplexExpand` (treating `t` as real), and simplifies under the assumption `tmin < t < tmax`. It then checks whether the result minus `V` simplifies to zero.

### 7. The solution mesh: equipotentials, field lines and shading

Once every surface passes, the potential is `Φ = Re W(x + i y)` and the field lines are level curves of `Ψ = Im W`. In the final plane these are simply Cartesian coordinates, so the tool builds the mesh **there**, where it is trivial, and pulls it back:

1. **Lines.** The equipotentials are the vertical lines `Re w = level`, evenly spaced between the lowest and highest conductor potentials (widened if the potential inside the view box goes beyond them). The field lines are the horizontal lines `Im w = level`, evenly spaced across the range `Im w` takes in the region.
2. **Cells.** The final-plane rectangle is divided into square cells whose edges include every one of those lines. Each block between neighbouring lines is split into several cells. The checkerboard colours whole blocks.
3. **Pulling back.** Each cell's centre is pulled back to the z-plane with a numerical inverse: Newton's method on `W(z) = w`, started from the six nearest forward-mapped points of the region. The cell is kept if its centre lands in the region; its four corners are then solved starting from the centre. A map that is several-to-one, like `z^2 - 1`, simply produces one copy of the cell for each preimage that lies in the region.
4. **Pushing forward.** The pulled-back corners are mapped through every plane like everything else, so the same mesh is drawn at every stage.

The result is Maxwell's net of **curvilinear squares**: shading, equipotentials and field lines all come from one mesh. In the final plane it is an exact rectangular grid. In the z-plane its cells are small where the field is strong, near edges and singularities, and large where it is weak, so resolution goes where it is needed without any refinement. No `ContourPlot` is involved, and the equipotentials and field lines are exact (to Newton's accuracy) rather than interpolated.

The numerical inverse replaces version 1's symbolic inverse maps and branch menus. The branch is chosen by the region itself, through the seeds: the preimage found is always one that lies in the region. If no cell can be pulled back (the inverse fails to converge), the tool says so and falls back to the even grid.

Before the problem is solved there is no natural mesh, so the shading and the grid lines come from an even grid in the z-plane, refined where some plane stretches it (step 3).

## Branch cuts, poles and the point at infinity

- A **branch cut** running through the region is not an error: the map is still analytic on each side. It tears the region in the next plane, and the pieces fit together along the cut. `Log` on an annulus and `ArcCosh` on an elliptical annulus both do this. What matters is that the *potential* `Re W` is continuous across the cut. The check table reports a cut that crosses a *conductor* if it spoils that conductor's line.
- A **pole** of a map inside the region sends nearby points far away. The tool drops points beyond 10⁸, and the plot window ignores the most extreme points. A conductor passing through a pole, as in the touching-cylinders example, becomes an infinite line; its samples near the pole are simply skipped in the check.
- **Infinity** is not a point the tool can draw. For regions that extend to infinity, the edge of the view box stands in for "far away". Its image shows where the far region goes, for instance into the small hole near `w = 1` in the *Two parallel wires* example.

## The limitations listed for version 1

The 2024 write-up of version 1.0.4 ("What if Maxwell had Mathematica?") lists a number of known limitations. This is how version 2 handles each of them:

| Limitation in v1 | Version 2 |
| --- | --- |
| A. A parametrisation that crosses a pole draws lines out to ±∞, so the `t` range had to start and end at the singularity | Lines are broken wherever neighbouring points jump far apart, and points beyond 10⁸ are dropped, so the `t` range can start anywhere |
| B. Branch cuts: surfaces had to start on the cut, and only one cut was possible | The same line breaking handles any number of cuts |
| C. One inverse branch had to serve the whole surface | The inverse is computed numerically (Newton's method) from points known to be in the region, so the right preimage, or every preimage of a several-to-one map, is found without choosing branches |
| D. Plot quality near singularities needed manual PlotPoints/recursion tuning | Adaptive refinement (above), on by default |
| E. Shading interpolated linearly from Surface 1 to Surface 2, with no guarantee it stayed in the region | Shading is made of cells tested against the region itself; once solved, it is the pulled-back net of equipotentials and field lines (step 7) |
| F. Surfaces had to be nested | Roles (conductor inside the region, outer boundary, open curve) allow any arrangement |
| G. Every surface needed a known potential | A surface can be marked as having an unknown potential |
| Future work: field lines, shading by field lines, single-point transformation, exclusions | All are in: the shading and grid follow the field lines and equipotentials (step 7), a point can be followed through every plane, and line breaking replaces exclusions |

One behaviour noted in the paper remains, by design: during the animation the shading can briefly cross a boundary, because the in-between pictures are straight-line blends, not conformal images. The pictures at whole-number stages are exact.

## Limits of the numerical approach

- Fine features smaller than the sampling (400 points per surface, a 61 × 61 grid before solving, about 61² mesh cells after) are not resolved. Use the *Shading resolution* setting for finer cells.
- A conductor that passes the numerical check is right to about six digits at the sampled points. Use the exact check for a proof.
- The tool cannot tell whether a map is one-to-one on the region. A map that folds the region over itself, like `z^2` on a region surrounding the origin, still produces pictures. The boundary check can still pass, as with `z^2 - 1` in the *Two wires inside a shell* example. There the folding is harmless: the potential is a function of `z^2`, which is single-valued.
