# Jelly Slice

A calm, ASMR-style jelly cutting game that runs entirely in the browser. No build step, no dependencies, no assets: open `index.html` and play.

## Play

| Action | How |
| --- | --- |
| Jiggle / drag the jelly | Left-drag on it (Grab tool) |
| Slice | Switch to the Knife (`K`, or the toolbar) and drag through the jelly, **or** hold the right mouse button anywhere |
| Swap tools | `G` grab, `K` knife, `Space` toggles |
| New jelly | `R` or the toolbar (also cycle shape with `1`-`4`, flavour with the colour button) |
| Mute | `M` |

Cut a piece as many times as you like; every cut produces real, independent pieces that you can drag around, stack and cut again. Slow, gentle strokes sound the nicest.

## How it works

* **Soft body** – each piece is a triangle mesh simulated with position-based dynamics (XPBD, small sub-steps): edge constraints for shear/stretch, per-triangle area constraints for near-incompressible volume, nonlinear stiffening so it resists folding, internal velocity damping so it wobbles instead of buzzing.
* **Grabbing** – a soft anchor chases the pointer (critically damped spring) and pulls a smoothly weighted patch of vertices, so the jelly lags, stretches and wobbles back instead of being yanked.
* **Cutting** – the knife is an exact geometric crack: the stroke is walked triangle-by-triangle through the *deformed* mesh, edges are split where it crosses (or snapped onto a vertex it passes very close to), the vertices along the path are duplicated using the mesh topology to decide which side each triangle belongs to, and disconnected components become new independent pieces. Rest positions are interpolated at the new vertices, so each piece relaxes toward the correct shape. Slits that stop mid-jelly stay open as real cracks, and later strokes continue from the tip.
* **Contacts** – pieces collide with each other (boundary vertex vs. boundary polygon with a small skin), and with the plate (Coulomb friction).
* **Look** – layered canvas 2D: extruded top face, translucent gradient body, subsurface glow, darker saturated rim, glossy rim highlights, and a specular streak that is attached to the mesh in rest space so it wobbles with the jelly.
* **Sound** – synthesized live with the Web Audio API (band-passed noise for the slice, thumps for landings, squishes for grabs).
