# Conformal Mapping Tool

An interactive Mathematica notebook for learning how **conformal maps solve two-dimensional electrostatics problems**. While developed with electrostatics in mind, these same techniques can transfer over to cartography and map projections, fluid mechanics and aerodynamics, heat transfer and potential flows, general relativity, quantum field theory, and conformal field theories.

You describe a set of conductors (curves held at fixed potentials) and then build a chain of conformal maps one step at a time. The tool carries the whole region between the conductors through every map, animating each step, so you can *see* what a Möbius transformation, a logarithm or a Joukowski map actually does to a region. When every conductor has been mapped onto a straight line `Re w = V`, the boundary-value problem is solved. The tool then shows the potential `V(x, y)`, draws the equipotentials and field lines, and carries them back through every plane.

It is meant for:

- **Students** in electromagnetism or complex-variables courses who want to experiment with the method of conformal mapping and check their own solutions.
- **Instructors** who want a live demonstration of why the method works, or worked examples to build homework around.

## What you can do with it

- **Describe almost any arrangement of conductors.** Give each conductor a parametric curve `x(t), y(t)`, a range for `t` and a potential `V` (a constant, or a function of `t`). Mark it as a conductor inside the region, as the outer boundary of the region, or as an open curve such as a plate or a semi-infinite sheet. Conductors can sit side by side: there is no need for each surface to be nested inside the previous one.
- **Restrict the region** with an inequality such as `y > 0` or `x > 0 && y > 0`. This is how half-planes, wedges, corners and strips bounded by open plates are described.
- **Build the solution one map at a time.** Type each map `w(z)` or pick one from a menu of common maps (Möbius, logarithm, exponential, powers, square root, Joukowski and its inverse).
- **Watch the region transform.** A single animation slider runs from the z-plane through every intermediate plane to the final one. The region is shaded (optionally as a checkerboard, which shows the angle-preserving property nicely), and a coordinate grid is carried along with it.
- **Follow a single point.** Pick a point on a surface with a slider and watch where it goes in every plane, with a readout of `z ↦ w1 ↦ w2 ↦ …`.
- **Leave a potential unknown.** Untick *known* for a surface you only want to watch. It is mapped and drawn but left out of the check, and the table tells you which line it lands on.
- **Check the boundary conditions live.** A table shows, for every conductor, the value of `Re w` (or `Im w`) along its image in the final plane, and whether it matches the required potential. An *exact check* button also verifies each condition symbolically.
- **See the solution.** Once every check passes, the tool shows the composite map `W(z)` and the potential `V(x, y)`. It draws colour-coded equipotentials and dashed field lines in the z-plane, the final plane and every plane in between, and the shading turns into Maxwell's net of curvilinear squares between them. The mesh is built in the final plane, where it is a rectangular grid, and pulled back through a numerical inverse of the maps.
- **Start from worked examples:** parallel plates, coaxial cylinders, touching (eccentric) cylinders, confocal elliptic cylinders, a right-angle corner, two parallel wires, and two wires inside a shell.

## Getting started

1. Download `ConformalMappingTool.nb` and open it in Mathematica (built for Mathematica 14; see [Requirements](#requirements)).
2. Click the input cell `ConformalMapTool[]` under **Start here** and press <kbd>Shift</kbd>+<kbd>Enter</kbd>.
3. If Mathematica asks whether to evaluate the notebook's initialization cells, choose **Yes**. If it asks whether to enable dynamic content, choose **Enable**.
4. The tool opens on the coaxial-cylinders example. Press the play button under the right-hand plot and watch the annulus become a rectangle.

To open the tool on a different example, evaluate for instance

```mathematica
ConformalMapTool["Two wires inside a shell"]
```

or use the **Load example** menu inside the tool. `ConformalMapTool["Blank"]` starts with a single conductor and the identity map.

## A first walk-through: coaxial cylinders

The example has an inner cylinder `|z| = 1` at `V = 0` and an outer cylinder `|z| = 2` at `V = 1`.

1. The left plot is the physical region (the z-plane). The shaded annulus is the region where we want the potential.
2. The maps are `w1 = Log[z]` and `w2 = z/Log[2]`. Drag the animation slider from 0 to 1. `Log` unrolls the annulus into a rectangle: circles centred on the origin become vertical lines. The cut along the negative real axis becomes the top and bottom edges.
3. From 1 to 2, the rescaling moves the outer cylinder to `Re w = 1`.
4. The check table now shows both conductors satisfied: `Re w = 0` on the inner one and `Re w = 1` on the outer one. The potential appears (it simplifies to `log(x² + y²)/(2 log 2)`), together with circular equipotentials and radial field lines.

Try breaking it. Change `w2` to `z/2`, and the outer cylinder's row turns red and reports how far off it is. Then try fixing it yourself.

## Why the method works (in one paragraph)

If `W(z)` is analytic in a region, its real part `Re W(x + i y)` automatically satisfies Laplace's equation there. Composing analytic maps keeps the result analytic, so any chain `W = wn ∘ … ∘ w1` gives a harmonic function. If, in addition, `Re W` equals the prescribed potential on every conductor, then `Re W` *is* the electrostatic potential, because the solution of Laplace's equation with given boundary values is unique. The tool's job is to make the second condition easy to reach and easy to check: you choose maps until each conductor lands on a vertical line `Re w = V` in the final plane. The level curves of `Im W` are then the field lines. See [docs/how-it-works.md](docs/how-it-works.md) for the details, including what the tool computes numerically.

## Documentation

- [User guide](docs/user-guide.md): every control, the plots, the check table, the warnings, and tips for describing curves and regions.
- [How it works](docs/how-it-works.md): the mathematics, and how the tool samples, tests, maps and checks.
- [Examples and exercises](docs/examples.md): what each built-in example demonstrates, plus exercises for students.
- [Changelog](CHANGELOG.md): what changed from version 1.0.4.

## Requirements

- Mathematica (the desktop front end; the tool is an interactive notebook). Version 2.0.0 was written for Mathematica 14, the version that version 1.0.4 was saved with. It uses only standard built-in functions, but it has not yet been tested on older versions.
- No packages or downloads beyond the notebook itself.

## Limitations worth knowing

- **The tool checks a solution; it does not find one.** Choosing the maps is the whole exercise. Whether a geometry can be solved with elementary maps is a mathematical question. For most shapes there is no known closed-form map: for example, two arbitrary wires at different potentials inside a shell.
- **Accuracy near poles.** Curves and shading cells that get stretched near a pole or the edge of the region are subdivided automatically. Very close to a pole, some shading is still dropped rather than drawn as huge polygons.
- **Principal branches.** `Log`, `Sqrt`, `ArcCosh` and powers use Mathematica's principal branches. A branch cut that runs through the region tears the shaded region apart in the next plane. That is often exactly what you want, as with `Log` on an annulus, but it can also hide a mistake.
- **Regions that reach infinity are shown truncated.** The view box is the size set by the *View size* slider. Its edge is carried through the maps like everything else.
- **Numerical check.** A conductor passes when `Re w` matches `V` within 10⁻⁶ (relative) at every sampled point that stays finite. The exact (symbolic) check is available on request, but it may time out on complicated maps.

## Credits

Created by Daniel Hooker (Fresno State). Version 2.0.0 is a rewrite of version 1.0.4 (see the [changelog](CHANGELOG.md)); the original notebook is kept in [`legacy/`](legacy/) for reference.

## License

[MIT](LICENSE). You are free to use, adapt and share the tool, including in courses; please keep the copyright notice.
