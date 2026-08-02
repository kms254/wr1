# WR1 branding guide

Two color systems ("brands") for the WR1 tools in this repo. Each is a full
light/dark pair behind the same CSS custom-property names, so any page that
already uses the tokens below gets both brands and both color schemes for
free — nothing to rewrite per-component.

Live reference with swatches, type, and component examples: **[`/brand/`](shopping-list/brand/index.html)**
(deployed at `https://kms254.github.io/wr1/brand/`).

## Track — default

Blue / black / grey. One livery, meant to read the same whether the car
underneath it is the black build or the blue one — this is the shared brand,
not either car's individual color. Confident, cool-toned, a little
dashboard-like: flat blue accent (no gradients), a genuine near-black ground
in dark mode rather than a tinted grey, uppercase mono labels for anything
that's data.

| Token | Light | Dark | Use |
|---|---|---|---|
| `--ink` | `#14171c` | `#eef1f5` | Primary text |
| `--ink-soft` | `#565f6b` | `#99a2ae` | Secondary text, captions |
| `--paper` | `#eef0f3` | `#0c0f13` | Page background |
| `--card` | `#ffffff` | `#171b21` | Card / row background |
| `--line` | `#d6dae1` | `#2b313a` | Borders |
| `--line-soft` | `#e6e9ed` | `#1f242b` | Faint dividers, code backgrounds |
| `--accent` | `#1c5adb` | `#5b93ff` | Links, primary buttons, active state |
| `--accent-ink` | `#ffffff` | `#0b1220` | Text on `--accent` |
| `--accent-soft` | `#dde8fb` | `#1c2b46` | Accent-tinted panels |
| `--good` | `#3c7a54` | `#7fb88f` | Semantic: received / confirmed |
| `--warn` | `#c23b2c` | `#e2705a` | Semantic: needs attention |

`--good` and `--warn` are **semantic**, not brand accents — they mean
"arrived" and "needs attention" everywhere in both brands and don't shift
with the theme. Only `--accent` carries the brand's identity color.

## Curv — matches curvlab.com

Pulled from CURV LAB's actual site (curvlab.com — the studio behind the WR1
build this repo tracks), so this tool can sit next to their own pages
without clashing. Their real identity has **no accent color at all**: an
off-white ground, near-black text and nav, warm-grey secondary text, and
plain text-link CTAs rather than filled colored buttons — restrained by
design, "less-is-more." Curv reproduces that by making the "accent" itself
monochrome: buttons are ink-filled, not color-filled. Corners are also
tightened (6px vs. Track's 10px) for a more architectural, less-rounded
feel, matching their clean-grid, gallery-like layout.

| Token | Light | Dark |
|---|---|---|
| `--ink` | `#111111` | `#f2f1ee` |
| `--paper` | `#fafaf8` | `#0e0e0d` |
| `--card` | `#ffffff` | `#1a1a18` |
| `--accent` | `#111111` (= ink) | `#f2f1ee` (= ink) |
| `--good` | `#3f6b4c` | `#6f9c7d` |
| `--warn` | `#8a3b2e` | `#c17b68` |
| `--radius` | `6px` | `6px` |

`--good`/`--warn` are muted rather than the fuller saturation used in Track —
kept just visible enough to still read as semantic, without breaking the
monochrome mood.

## How the switch works

Every themed page sets tokens three ways, same pattern both brands follow:

1. `:root { ... }` — the default (Track, light).
2. `@media (prefers-color-scheme: dark) { :root { ... } }` — follows the OS/browser.
3. `:root[data-theme="dark"]` / `:root[data-theme="light"]` — forces a mode, overriding #2, for a manual light/dark toggle.

Curv repeats the same three rules scoped under `:root[data-brand="curv"]`.
Switching brands is just setting `data-brand="track"` (or removing the
attribute) vs `data-brand="curv"` on `<html>`. The shopping list's brand
switcher — a small pill in the top-right of the header — does exactly this
and remembers the choice in `localStorage` (`wr1-cart:brand`), shared with
`/brand/` so switching on either page carries over to the other.

## Typography

Track uses a system UI stack only — no custom webfonts, kept deliberately
plain so the data reads fast on a phone at a parts counter:

- **Text**: `ui-sans-serif, -apple-system, "Segoe UI", Roboto, sans-serif`
- **Data / numerals**: `ui-monospace, "Cascadia Code", "SFMono-Regular", Menlo, Consolas, monospace`, always with `font-variant-numeric: tabular-nums` so counts don't jitter as they change.

**Curv is the one exception**, by design — matching CURV LAB's real site
means matching their actual typeface. Their body copy is set in **Poppins**
(open-licensed via Google Fonts, loaded from `fonts.googleapis.com`); their
headings use **Clarkson**, a Squarespace-exclusive font we have no license
to use, so Curv substitutes **Poppins Bold** for headings too — close enough
in geometric character without borrowing something we can't ship. Both
brands share one `--font-sans` token, so this swap is a single declaration
inside the Curv block, not a page-wide change.

Headings are bold and tight (`font-weight: 800`, negative letter-spacing).
Labels and eyebrows are small, uppercase, and letter-spaced
(`0.08–0.14em`) — reserved for things that are actually labels (category
eyebrows, flags, badge text), not decoration.

## Components

Defined once, styled entirely off tokens above — see `/brand/` for live
examples of each:

- **Card** — `--card` background, 1px `--line` border, `--radius` corners
  (10px for Track, 6px for Curv), a soft two-layer shadow.
- **Flag / badge** — small uppercase pill, `--warn`/`--warn-soft` by default,
  `.good` modifier swaps to `--good`/`--good-soft` for a confirmed/positive
  state.
- **Segmented control** (`.seg`) — pill-shaped button group, active state is
  a solid `--accent` fill. Used for both the order-status filter and the
  brand switcher itself.
- **Primary button / link** — solid `--accent` fill, `--accent-ink` text.
- **Progress track** — two-tone fill (`--accent` for "ordered," `--good` for
  "received") so the *gap* between the two fills reads as "in transit"
  without a third color.

## Adding a third brand

Copy the Curv block, rename `data-brand="curv"` to your new key, pick new
values for the twelve tokens above (keep `--good`/`--warn` semantic — don't
reassign their meaning; only override `--font-sans` or `--radius` if the
brand genuinely calls for a different typeface or corner treatment), and add
a button to `.brand-seg` with a matching `data-brand-choice`. Nothing else
needs to change.
