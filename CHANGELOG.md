# Changelog

## 2.0.0 (2026-11-04)

A rewrite of version 1.0.4. The notebook now has a title page, instructions, launch cells for the examples, and the implementation in clearly separated initialization cells. Previously everything was in one large cell.

### Added

- **Regions with conductors side by side.** Each surface now has a role: *conductor inside the region*, *outer boundary of the region*, or *open curve*. The region is everything inside the outer boundary (if any) and outside every conductor, so several conductors can sit next to each other inside a shell, or in an unbounded region. Version 1 assumed that each surface was nested inside the previous one.
- **Open boundaries and region restrictions.** Plates and other open curves are supported, and an optional condition such as `y > 0` describes half-planes, strips and wedges.
- **Field lines**, alongside the equipotentials.
- **A solution mesh of curvilinear squares.** Once solved, the equipotentials, field lines and shading all come from one grid built in the final plane and pulled back through a numerical inverse (Newton's method seeded from the region), so the grid follows the field lines exactly and its resolution concentrates near edges and singularities.
- **A live, per-surface boundary-condition table.** It shows the actual range of `Re w` (or `Im w`) on each conductor and how far it is from the required potential.
- **An exact (symbolic) check**, available on request.
- **One animation for the whole chain of maps.** It runs from the z-plane to the final plane, with a smoothly interpolated plot window and buttons to jump to any plane.
- **The region is shaded in every plane**, optionally as a checkerboard, and a coordinate grid is carried through the maps with it.
- **Equipotentials and field lines in every plane**, not only the z-plane.
- **The composite map `W(z)` and the potential `V(x, y)`** are displayed once the problem is solved.
- **Adaptive refinement** near poles and region edges, replacing the manual plot-performance tuning (limitation III.D of the 2024 write-up).
- **Unknown potentials:** a surface can be left out of the check, and the table reports which line it lands on (limitation III.G).
- **Follow a point:** a marker on the selected surface is carried through every plane, with a readout of its position in each (future work IV.B).
- **Seven worked examples**, a *Blank* starting point, and a menu of common maps.
- **Warnings** for undefined symbols, invalid parameter ranges, open curves marked as closed conductors, conductors outside the outer boundary or inside each other, an empty region, and maps that send the whole region to infinity.
- `ConformalMapTool[]` and `ConformalMapTool["example name"]` to open the tool.

### Changed

- **Inverse maps are computed numerically.** Version 1 pulled the final-plane lines back through symbolic inverses with a user-chosen branch, which assumed nested surfaces and one branch per surface. Version 2 inverts the composite map with Newton's method, started from points known to be in the region, so the branch is chosen by the region and every preimage of a several-to-one map is found. The inverse-map selector is gone.
- **The boundary condition is checked numerically** at 400 points per surface, with an optional symbolic check, instead of with `Reduce`. `Reduce` can fail to confirm a correct map, and can be slow.
- **Regions that reach infinity** are handled with a "View size" setting instead of the special *Origin* and *Infinity* surfaces. Those surfaces are replaced by the outer-boundary role: "the inside of S1" is now S1 as the outer boundary, and "the outside of S1" is S1 as a conductor with no outer boundary.
- **The choice of potential** ("f: x / y") is now "conductors become lines of constant Re w / Im w".
- **The plot-tuning controls** (PlotPoints, MaxRecursion, Mesh) are replaced by resolution menus.
- **All state is local to each copy of the tool.** Two copies no longer share surfaces or maps, and nothing leaks into the global context.

### Fixed (problems in 1.0.4)

- The known limitations listed in section III of the 2024 write-up are addressed; see the table in [docs/how-it-works.md](docs/how-it-works.md#the-limitations-listed-for-version-1).

- The "is this curve closed?" test never worked. `t` was a local variable of the outer `DynamicModule`, so `sx /. t -> to` never replaced the `t` typed into the input fields, and the test passed for any curve.
- The surface menu was disabled whenever the selected surface had an empty `t` range, and *Check V(w(t))* only checked that the *selected* surface was closed.
- `remap` was called with the wrong number of arguments in the `x(t)`, `y(t)` and `V` tracking functions. Those calls did nothing.
- In the transformation plot, the upper `t` limit used `Max[trange[[S1]][[1]], …]`, the lower end of S1's range, instead of `[[2]]`. That plot also indexed `trange[[S2]]` directly, which failed when S2 was *Origin* or *Infinity*. Both interpolation endpoints of the `t` range were identical, so the interpolation did nothing.
- *Reset* set `surfaces = {{0, 0}}` and `plots = {{0, 0}, {0, 0}}`, which do not have the structure the rest of the code expects, so the tool broke after a reset. It also left the inverse lists and other state unchanged.
- `Enabled -> Not[autopp]` (and `Not[autorec]`) lacked `Dynamic`, so the PlotPoints and recursion fields never became enabled.
- Debugging labels ("wererer", "wer") appeared in the final-map plot titles.
- `SetSharedVariable` (a parallel-computing function) was called for no reason, along with other leftovers (`timep`, `If[w === 0, …]`). The `time = 1 - time; time = 1 - time` trick for forcing redraws is no longer needed.

## 1.0.4

The original single-cell `Manipulate` version.
