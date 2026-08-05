# wr1

WR1 CURV LAB chassis build — sourcing, build steps, and notes for a
parent-and-kid RC car project.

**Live site: <https://kms254.github.io/wr1/>**
· Repo: <https://github.com/kms254/wr1>

Everything here is a static site: plain HTML, one shared stylesheet, no build
step, no backend, no dependencies. Checkbox and preference state saves to your
browser's `localStorage`.

## Pages

| Page | What it is |
|---|---|
| [Shopping list](https://kms254.github.io/wr1/) — [`shopping-list/index.html`](shopping-list/index.html) | Sourcing and delivery tracker. Every off-the-shelf part, fastener, tool, and consumable the guide calls for, grouped by category (printing, drivetrain, suspension, pivots, fasteners, fluids, tools, optional), with per-item **Ordered** / **Received** checkboxes. |
| [Build guide](https://kms254.github.io/wr1/build/) — [`shopping-list/build/index.html`](shopping-list/build/index.html) | The build itself in 16 sessions, from pressing the chassis-plate inserts through the ESC/battery/receiver install and the optional FPV mount. Photos, per-step checkboxes, safety notes, and check-in questions. |
| [Parent guide](https://kms254.github.io/wr1/parent-guide/) — [`shopping-list/parent-guide/index.html`](shopping-list/parent-guide/index.html) | What the build actually costs (~$619 all-in, set against the $170–$1,200 a week of STEM camp runs) and what a kid picks up along the way. |
| [ESC programming](https://kms254.github.io/wr1/programming/) — [`shopping-list/programming/index.html`](shopping-list/programming/index.html) | Throttle-range calibration, LED program-card settings, and factory reset. |
| [Brand guide](https://kms254.github.io/wr1/brand/) — [`shopping-list/brand/index.html`](shopping-list/brand/index.html) | Live swatches, type, and component examples for both color systems. See [`BRANDING.md`](BRANDING.md) for the full write-up. |

## Running it locally

Open any `index.html` directly in a browser, or serve the folder statically so
the relative links between pages resolve the same way they do when deployed:

```sh
python3 -m http.server -d shopping-list 8000
# then visit http://localhost:8000
```

## Themes and brands

Every page supports light/dark (following the OS by default, with a manual
toggle that overrides it) and two brands — **Curv**, the default, matching
curvlab.com's restrained monochrome identity, and **Track**, a blue/black/grey
alternate. The brand switcher lives in the page header and remembers your
choice across pages. Tokens, component styles, and how to add a third brand
are in [`BRANDING.md`](BRANDING.md).

## Deploying

[`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml)
publishes the `shopping-list/` directory to GitHub Pages on every push to
`main` that touches it, and can also be run manually via **workflow_dispatch**.

## Repo layout

```
shopping-list/         # site root — what gets deployed
  index.html           # shopping list
  theme.css            # shared tokens, both brands, light + dark
  build/               # 16-session build guide (+ images/)
  parent-guide/        # cost and learning write-up
  programming/         # ESC programming guide
  brand/               # live brand reference
  images/              # part photos for the shopping list
BRANDING.md            # design system write-up
docs/plans/            # working notes and change plans
.claude/agents/        # review agents used on this repo
```
