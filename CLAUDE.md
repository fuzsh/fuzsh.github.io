# CLAUDE.md

Personal academic site for Farzad Shami (fuzsh.github.io). Two things live here:

1. **`index.html`** — an explorable 3D Helsinki, GTA-style, where each landmark
   opens a piece of the CV. One self-contained file, ~2300 lines.
2. **A Jekyll site** — the classic HTML pages (`publications.html`, `cv.html`,
   `experience.html`, `contact.html`) with `_layouts/`, `_includes/`, `css/`.
   See `STRUCTURE.md`. Deployed by GitHub Pages from `main`.

`index.html` is the landing page and is **not** a Jekyll page — it has no front
matter and must not gain one. Everything else goes through Jekyll.

## Running it

```bash
python3 -m http.server 8899        # then open http://127.0.0.1:8899/index.html
bundle exec jekyll serve           # for the Jekyll pages
```

Opening `index.html` with `file://` works too; there are no fetches.

## index.html — ground rules

- **Single file, zero build.** Three.js comes from a CDN as an ES module. There
  is no bundler, no npm, no `assets/` dependency for the 3D scene — every
  texture is painted into a `<canvas>` at load. Keep it that way; a build step
  here buys nothing and breaks GitHub Pages' zero-config deploy.
- **Only optional external files** are `assets/photo.jpg` and
  `assets/files/cv.pdf`, both referenced with graceful fallbacks.
- **Deterministic city.** `_s = 20260803` seeds an LCG (`rr`, `rng`, `rpick`).
  The city is identical on every reload. Changing the seed reshuffles every
  building, so don't change it casually.
- Numbered section banners run 1–19 top to bottom. Add code to the section it
  belongs to rather than appending at the end.

## Editing the CV content

All text lives in the `POIS` array at the top of the `<script>` (section 1).
Each entry is `{id, name, icon, pos:[x,z], r, title, sub, body}` where `body` is
an HTML string and `pos` is the world position its marker beacon sits at. Adding
a POI only adds a marker — the *building* is placed separately in sections 7–10,
so a new landmark needs both.

Content shown in the 3D page is duplicated in the Jekyll pages. If you change a
publication or a date, change it in both.

## Physics

Two simulations, both in SI units. Prefer fixing the *parameters* over adding
special cases — the whole point is that behaviour falls out of the model.

**Vehicle (`stepCar`, `CAR`)** — a single-track "bicycle" model: per-axle slip
angles, saturating tyre forces capped by the friction circle
(`tyre()` uses `μFz·tanh(Cα/μFz)`), longitudinal weight transfer setting the
axle loads, aero drag and rolling resistance. Understeer, snap oversteer and
handbrake drifts are emergent, not scripted. Body roll and pitch are driven from
the computed accelerations and applied to a `chassis` sub-group so the wheels
stay planted.

Sign conventions, which are easy to get backwards:
- forward `e_f = (sin ψ, cos ψ)`, right `e_r = (cos ψ, −sin ψ)`, `ψ = rotation.y`
- `S.u` is longitudinal velocity, `S.v` lateral (positive = sliding right),
  `S.r` is yaw rate; positive steer and positive yaw rate both turn right.

**Character (`stepPlayer`, `poseWalk`, `legIK`)** — force-based locomotion
(target velocity, finite acceleration, ground friction, weak air control), and a
skeleton posed by two-link inverse kinematics rather than by swinging rigid
limbs. The stride length is *derived*, not chosen: over the stance phase the
planted foot has to travel backwards under the hips at exactly walking speed, so
`strideLen = 2·halfStep / DUTY`. This is what stops the feet skating; if you
change leg length, hip height or duty factor, the stride follows automatically.

The figure faces local **+z**, so on every joint a **positive `rotation.x` swings
the segment backwards**. `poseWalk` works in anatomical terms (`th` = thigh
forward, `kf` = knee flexed) and negates at the transform. Getting this backwards
gives hyperextending knees that are hard to spot at gameplay distance — which is
what the self-check exists for.

## Self-check

```
http://127.0.0.1:8899/index.html?selftest      # results go to the console
```

Section 18 drives the real `stepCar` and `poseWalk` and asserts on the outcome:
acceleration, coast-down, braking, straight-line stability, turn radius, the
friction-circle limit, handbrake oversteer, reverse — and, for the walk and the
run, that knees only flex backwards, feet never sink through the ground, the
swing foot clears it, and the planted foot does not skate.

Run it after touching either simulation. The assertions encode physics, so a
failure usually means the model is wrong, not that the threshold needs widening
— check the reported number against what the real quantity should be first.

## Facade textures

Buildings are generated against a real storey module: `STOREY = 3.4 m`,
`MODULE = 6.8 m`, one texture tile = 2 bays × 2 storeys. Building heights and
widths are snapped to that module (`snap()`) and `boxUV` starts `v` at the
building's base, so storeys line up with the ground instead of floating at an
arbitrary phase. **If you change `MODULE`, the snapping must change with it.**

Four styles in `FSTYLE`, laid out by distance from the centre (`districtStyle`),
matching how Helsinki actually layers outward:

| Style | Where | What it reproduces |
|---|---|---|
| `empire` | historic centre | Engel's 1820–40 Senate Square: lime render in pale ochre/yellow, white painted trim, per-floor string course, tall six-light windows |
| `jugend` | inner ring | 1900–15 Katajanokka/Eira: granite rustication, roughcast render in grey-green and warm grey, arched heads |
| `brick` | inner/outer ring | red and yellow Finnish brick, white-painted surrounds |
| `funkis` | outskirts | 1930s functionalism: smooth white render, wide low windows |

Roofs are seamed sheet metal (`seamRoofTex`) in iron-oxide red (*punamulta*),
dark green and graphite, with copper patina reserved for landmarks.

Cost control, since these are painted at load: the bump/roughness map depends on
the style's geometry and not the paint colour, so it is generated **once per
style** and shared; the emissive night-window mask is only rectangles and runs at
128². Two colour variants per style keep neighbouring buildings from repeating.
Adding variants or raising resolutions multiplies texture memory quickly —
measure before you do.

## Gotchas

- `resolve(p, radius)` is AABB push-out against `colliders` and mutates `p` in
  place. Both `stepPlayer` and `stepCar` diff the position before and after to
  recover the contact normal and cancel velocity into it. Anything new that
  moves needs the same treatment or it will stick to walls.
- Shader uniforms are shared: `skyUni.sunDir` is the *same object* as
  `seaUni.sunDir`, and `applyLighting` mutates it.
- Materials tagged `userData.lit` are collected into `litMats` by a single
  `world.traverse` (section 11) and get their `emissiveIntensity` driven by the
  day/night blend. A material created after that traversal will never light up.
- GLSL here is compiled by ANGLE and is stricter than it looks — a redeclared
  variable silently killed the sea shader until it was compiled headlessly.
- `occluders` feeds the camera-collision raycast and only takes meshes with
  volume > 14 m³, so small props don't push the camera around.

## Verifying visually

There is no test runner. For rendering changes, drive the page headlessly with
Chrome (`puppeteer-core` against `/Applications/Google Chrome.app`) and
screenshot it — the scene is deterministic, so shots are comparable across runs.
Dispatch synthetic `keydown`/`keyup` with the right `code` to move; input is read
from a `keys` map, not from focus.
