---

editor_options: 
  markdown: 
    wrap: 72
---

# CSS knobs cheat sheet — xaringan / remark.js

Reference for `custom.css` in this project. Organised by what you typically want to change. Each section: selector table → minimal recipe → notes.

------------------------------------------------------------------------

## How the stack is layered

xaringan loads CSS in this order (set in the YAML `css:` field):

``` yaml
css: ["default", "default-fonts", "custom.css"]
```

1.  `default.css` — slide layout, padding, h1/h2/h3, title-slide, slide numbers
2.  `default-fonts.css` — font family, weights, link colour
3.  `custom.css` — your overrides; loaded last so it wins

Most fixes live in `custom.css`. Rule of thumb: **later wins**, and **more specific wins**, so `.title-slide h1` beats plain `h1`.

The two big container classes:

- `.remark-slide-content` — every slide (including the title slide)
- `.title-slide` — only the title slide; wraps the same content

So `.title-slide h1` and `.remark-slide-content h1` both apply on slide 1; `.title-slide` is more specific and wins.

Read xaringan's own [default.css and default-fonts.css](https://github.com/yihui/xaringan/tree/master/inst/rmarkdown/templates/xaringan/resources) once — they're short and define every class you'll override.

------------------------------------------------------------------------

## 1. Title slide

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Big title text | `.title-slide h1` | `font-size`, `font-weight`, `color`, `line-height` |
| Subtitle text | `.title-slide h2` | `font-size`, `font-weight`, `color` |
| Author / date | `.title-slide h3` | `font-size`, `color` |
| Background colour | `.title-slide` | `background` |
| Background image | `.title-slide` | `background-image: url(...)`, `background-size: cover` |
| Centre everything | `.title-slide` | `text-align: center` |
| Centre a `max-width` box | `.title-slide h1` | `margin: 0 auto` |
| Hide slide-number on title | `.title-slide .remark-slide-number` | `display: none` |
| Vertical positioning | `.title-slide` | `display: flex; flex-direction: column; justify-content: center` |

**Minimal centred recipe:**

``` css
.title-slide {
  background: #17324d;
  color: white;
  text-align: center;
}
.title-slide h1 {
  color: white;
  font-size: 54px;
  line-height: 1.1;
  max-width: 920px;
  margin: 0 auto 22px auto;     /* centres the box */
}
.title-slide h2 { color: #d9e7f2; font-size: 28px; font-weight: 400; }
.title-slide h3 { color: #d9e7f2; font-size: 22px; font-weight: 400; }
.title-slide .remark-slide-number { display: none; }
```

**Gotcha:** `max-width` without `margin: 0 auto` looks left-aligned because the shrunk box doesn't centre itself, even if its inner text is `text-align: center`.

YAML pairs to populate these: `title:`, `subtitle:`, `author:`, `date:`. Split title and subtitle so they get separate h1/h2 styling.

------------------------------------------------------------------------

## 2. Regular slide titles

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Slide title size (`## ...`) | `.remark-slide-content h2` | `font-size` |
| Slide title colour | `.remark-slide-content h2` | `color` |
| Space below title | `.remark-slide-content h2` | `margin-bottom` |
| Underline / accent bar | `.remark-slide-content h2` | `border-bottom`, `padding-bottom` |
| Title in special slide colour | `.inverse h2`, `.section-slide h2` | `color` |

``` css
.remark-slide-content h2 {
  font-size: 1.7em;             /* or absolute, e.g. 36px */
  color: #0f766e;
  margin-top: 0;
  margin-bottom: 0.6em;
  border-bottom: 3px solid #d8e0e7;
  padding-bottom: 0.2em;
}
```

In Rmd, `## My Title` becomes the slide's h2. `# Title` is h1 (used on title slide and on `.inverse` / section dividers).

### Distance between slide title and content

This is a frequent tweak and there are *four* sources of vertical space between the top of a slide and the body. Diagnose top → bottom:

| Gap | Set by | Default-ish |
|------------------------|------------------------|------------------------|
| Slide top edge → top of title | `.remark-slide-content { padding-top: ... }` | \~52 px in your CSS, \~20 px in xaringan default |
| Title's own top breathing room | `.remark-slide-content h2 { margin-top: ... }` | usually `0` if you've set it; otherwise browser default `~0.83em` |
| Title → first line of content | `.remark-slide-content h2 { margin-bottom: ... }` | the main knob |
| Content's own top margin | `.remark-slide-content p:first-child, ... ul:first-child { margin-top: ... }` | browser default `~1em` |

The last two **margin-collapse** (the larger of the two wins, they don't add). The `padding-top` on `.remark-slide-content`, however, does *not* collapse with the h2's `margin-top` — padding never collapses with margin. So `padding-top` and `margin-top` always add.

**To tighten title-to-content:**

``` css
.remark-slide-content h2 {
  margin-top: 0;
  margin-bottom: 0.3em;          /* was 0.6em */
}
.remark-slide-content h2 + p,
.remark-slide-content h2 + ul,
.remark-slide-content h2 + ol,
.remark-slide-content h2 + .pull-left,
.remark-slide-content h2 + .pull-right {
  margin-top: 0;                  /* kills the content's own top margin */
}
```

The `h2 + selector` rules target the element immediately after the title — they neutralise its inherited `margin-top` so only the h2's `margin-bottom` matters.

**To loosen title-to-content** (more air around the title):

``` css
.remark-slide-content h2 {
  margin-top: 0.2em;
  margin-bottom: 0.8em;
  padding-bottom: 0.25em;
  border-bottom: 3px solid #d8e0e7;   /* the visible separator helps the eye */
}
```

**Why your slide looks unevenly spaced despite `margin-bottom: 0.6em`:**

- Common cause #1: the first content element (often a `<p>` or `<ul>`) brings its own `margin-top` ≈ `1em`. Margin-collapse means you see the bigger of `0.6em` and `1em` → `1em`. Reset content's `margin-top: 0` to fix.
- Common cause #2: `.pull-left` / `.pull-right` are floated. **Floats don't participate in margin-collapse**, so the column's `margin-top` adds on top of the h2's `margin-bottom`. Reset via the `h2 + .pull-left` rule above.
- Common cause #3: a leading blank line in the Rmd inserts an empty `<p>`. Remove the blank line, or add `.remark-slide-content p:empty { display: none; }`.

**Quick "good defaults" snippet:**

``` css
.remark-slide-content { padding: 36px 64px 42px 64px; }
.remark-slide-content h2 {
  margin: 0 0 0.45em 0;
  padding-bottom: 0.15em;
  border-bottom: 2px solid #d8e0e7;
}
.remark-slide-content h2 + * { margin-top: 0; }
```

------------------------------------------------------------------------

## 3. Body and inline text

| You want | Selector | Property |
|----|----|----|
| Base body font size | `.remark-slide-content` | `font-size` |
| Base font family | `body` or `.remark-slide-content` | `font-family` |
| Body text colour | `body` or `.remark-slide-content` | `color` |
| Bold weight | `strong` | `font-weight` |
| Italic colour | `em` | `color` |
| Link colour | `a` | `color`, `text-decoration` |
| Link hover | `a:hover` | `color`, `text-decoration` |
| Selection highlight | `::selection` | `background`, `color` |

``` css
body { font-family: "Inter", Arial, sans-serif; color: #1f2933; }
.remark-slide-content { font-size: 27px; }
a { color: #215c8f; text-decoration: none; }
a:hover { text-decoration: underline; }
::selection { background: #ffe58a; }
```

**Sizing tip:** body `font-size` ripples through *everything* using `em`. Set it once, override only outliers.

------------------------------------------------------------------------

## 4. Lists (bullets / numbers)

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Bullet style | `.remark-slide-content ul li` | `list-style-type` |
| Custom bullet character | `.remark-slide-content ul li::marker` | `content`, `color` |
| Indent | `.remark-slide-content ul` | `padding-left` |
| Gap between bullets | `.remark-slide-content li` | `margin-bottom` |
| Numbered list colour | `.remark-slide-content ol li::marker` | `color` |
| Tight list (override) | `.tight li` | `margin-bottom: 0` |

``` css
.remark-slide-content li { margin-bottom: 9px; line-height: 1.32; }
.remark-slide-content ul { padding-left: 1.3em; }
.remark-slide-content ul li::marker { color: #215c8f; }
```

------------------------------------------------------------------------

## 5. Tables

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Cell padding | `.remark-slide-content table td, .remark-slide-content table th` | `padding` |
| Header colour | `.remark-slide-content table th` | `color`, `background` |
| Row striping | `.remark-slide-content table tbody tr:nth-child(odd)` | `background` |
| Border (whole table) | `.remark-slide-content table` | `border` |
| Border (cells only) | `.remark-slide-content table td` | `border-bottom` |
| Whole-table font size | `.remark-slide-content table` | `font-size` |
| Small in-slide table | `.mini-table table` | `font-size`, `line-height` |
| Centre a column | `.remark-slide-content table td.center` | `text-align: center` |

``` css
.remark-slide-content table { border-collapse: collapse; font-size: 22px; }
.remark-slide-content table th {
  background: #f4f7f9; color: #215c8f; text-align: left;
  padding: 8px 12px; border-bottom: 2px solid #d8e0e7;
}
.remark-slide-content table td { padding: 6px 12px; border-bottom: 1px solid #eef2f5; }
.remark-slide-content table tbody tr:nth-child(odd) { background: #fafbfc; }
```

In Rmd, you can also wrap a table in `.font80[ ... ]` to scale it down ad-hoc.

------------------------------------------------------------------------

## 6. Two-column layouts

xaringan ships with `.pull-left` / `.pull-right` (47% each, floats). For more control, use CSS Grid:

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Two even columns | `.pull-left, .pull-right` | `width` (default 47%) |
| Three columns | `.cols-3` (custom) | `display: grid; grid-template-columns: 1fr 1fr 1fr` |
| Asymmetric columns | `.cols-7-3` (custom) | `grid-template-columns: 7fr 3fr` |
| Gap between cols | `.cols` | `gap` |
| Vertical alignment | `.cols` | `align-items: start \| center \| stretch` |
| Make a column wider | `.pull-left` | `width: 60%` (and `.pull-right { width: 35%; }`) |

``` css
.cols { display: grid; grid-template-columns: 1fr 1fr; gap: 28px; align-items: start; }
.cols-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 18px; }
```

Usage in Rmd:

``` markdown
.pull-left[
left column content
]

.pull-right[
right column content
]
```

For Grid:

``` markdown
.cols[
.col-a[ left ]
.col-b[ right ]
]
```

`.pull-left` uses floats so very tall content may push the next slide's header up. Grid avoids this.

------------------------------------------------------------------------

## 7. Code blocks

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Code font | `.remark-code, .remark-inline-code` | `font-family` |
| Inline code background | `.remark-inline-code` | `background`, `padding`, `border-radius` |
| Block code size | `.remark-code` | `font-size` |
| Block code background | `.remark-code` | `background`, `padding` |
| Highlighted line | `.remark-code-line-highlighted` | `background-color` |
| Hide line numbers | `.remark-code-line-number` | `display: none` |
| Code-block max height (scroll) | `.remark-code` | `max-height`, `overflow-y: auto` |

``` css
.remark-code, .remark-inline-code { font-family: "Source Code Pro", monospace; }
.remark-inline-code {
  background: #eef2f5; padding: 1px 6px; border-radius: 4px; font-size: 0.9em;
}
.remark-code { font-size: 22px; }
.remark-code-line-highlighted { background-color: #fff6cc; }
```

YAML `highlightStyle: github` controls the syntax-highlighting theme; valid values include `default`, `github`, `tomorrow`, `solarized-dark`, etc.

------------------------------------------------------------------------

## 8. Math (MathJax)

xaringan uses MathJax by default. Things to know:

| You want | Where | How |
|------------------------|------------------------|------------------------|
| Display equation size | `.remark-slide-content .MathJax` | `font-size` (use parent text size, MathJax scales relative) |
| Inline math | `` `\(x\)` `` (backticks) or `$x$` | in Rmd |
| Display math | `$$ ... $$` | in Rmd |
| Numbered equations | YAML | `nature: ratio:` etc. don't help — MathJax has its own equation numbering; usually off for slides |
| Avoid line-break issues | inline math | prefer `` `\(...\)` `` over `$...$` in xaringan |

To bump math size everywhere:

``` css
.remark-slide-content .MathJax { font-size: 105% !important; }
```

Use `!important` because MathJax inserts inline `font-size`.

------------------------------------------------------------------------

## 9. Figures and images

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Cap image width | `.remark-slide-content img` | `max-width` |
| Cap image height | `.case-figure img` (custom) | `max-height` |
| Centre an image | wrapping `<p>` or `.center[ ![]() ]` | n/a (use the inline class) |
| Round corners | `.remark-slide-content img` | `border-radius` |
| Drop shadow | `.remark-slide-content img` | `box-shadow` |
| Border | `.remark-slide-content img` | `border` |
| Placeholder box (no image yet) | `.placeholder` (custom) | dashed border + flex centre |

``` css
.remark-slide-content img { max-width: 100%; height: auto; border-radius: 4px; }
.case-figure img { max-height: 445px; object-fit: contain; }
```

Knitr chunks for live R figures take `fig.width`, `fig.height`, `out.width`, `out.height`, `fig.align`. Quick reference: `out.width="80%"` shrinks the embedded image to 80% of the slide width without resampling.

------------------------------------------------------------------------

## 10. Slide-level: padding, background, slide number, footer

| You want | Selector | Property |
|------------------------|------------------------|------------------------|
| Slide padding | `.remark-slide-content` | `padding` (top, right, bottom, left) |
| Slide background | `.remark-slide-content` | `background` |
| Slide-number colour | `.remark-slide-number` | `color`, `font-size` |
| Slide-number position | `.remark-slide-number` | `bottom`, `right` |
| Hide slide number | `.remark-slide-number` | `display: none` |
| Persistent footer | custom `.footnote` | `position: absolute; bottom: 24px; left: 64px` |
| Per-slide background | per-slide class | `background-image: url(...)` |

``` css
.remark-slide-content { padding: 52px 64px 42px 64px; background: #fbfcfd; }
.remark-slide-number { font-size: 14px; color: #94a3b8; right: 18px; bottom: 14px; }
.footnote {
  position: absolute; bottom: 24px; left: 64px; right: 64px;
  font-size: 16px; color: #5f6b7a;
}
```

Per-slide footer in Rmd:

``` markdown
some content

.footnote[Source: Sun et al. (2026)]
```

YAML `slideNumberFormat: "%current% / %total%"` controls what shows in the slide number.

------------------------------------------------------------------------

## 11. Inverse slides + section dividers

xaringan provides `.inverse`. You can define more (e.g. `.section-slide`).

| You want             | Selector         | Property                      |
|----------------------|------------------|-------------------------------|
| Inverse background   | `.inverse`       | `background`                  |
| Inverse text colour  | `.inverse`       | `color`                       |
| Inverse h2 colour    | `.inverse h2`    | `color`                       |
| Custom section slide | `.section-slide` | declare and use it on a slide |

``` css
.inverse, .section-slide { background: #17324d; color: white; }
.inverse h1, .inverse h2, .section-slide h1, .section-slide h2 { color: white; }
```

Usage in Rmd — declare the class on the slide separator line:

``` markdown
---
class: inverse, center, middle

# Part II — methods
```

Built-in helpers from xaringan: `center` (text-align: center), `middle` (vertical-centre), `left`, `right`, `top`, `bottom`, `inverse`.

------------------------------------------------------------------------

## 12. Inline class helpers (`.class[ ... ]`)

remark's `.class[content]` syntax wraps inline spans. Define small helpers in `custom.css` and reuse everywhere in Rmd.

Typical helpers:

``` css
.font50  { font-size: 50%; }
.font60  { font-size: 60%; }
.font70  { font-size: 70%; }
.font80  { font-size: 80%; }
.font90  { font-size: 90%; }
.large   { font-size: 130%; }
.xlarge  { font-size: 160%; }

.grey   { color: #777; }
.muted  { color: #999; }
.accent { color: #2E6E9E; }

.center  { text-align: center; }   /* shipped by xaringan default */
.right   { text-align: right; }
.tight   { line-height: 1.1; }

.callout {
  border-left: 6px solid #0f766e; background: #eef8f6;
  padding: 16px 22px; border-radius: 4px; margin: 22px 0 14px 0;
}
.warn { border-left-color: #b7791f; background: #fff7e8; }
```

Usage:

``` markdown
.font80.grey[Source: Sun et al. (2026)]

.callout[
**Key idea.** Partially shared variation lives between joint and individual.
]
```

Classes can stack: `.font80.grey[ ... ]` applies both.

------------------------------------------------------------------------

## 13. Using CSS variables for theming

Define once at `:root`, reuse everywhere — makes wholesale palette swaps trivial.

``` css
:root {
  --ink:  #1f2933;
  --blue: #215c8f;
  --teal: #0f766e;
  --accent: #2E6E9E;
}
body { color: var(--ink); }
a { color: var(--blue); }
.accent { color: var(--accent); }
```

To re-skin the deck for a different venue, change four hex codes at the top.

------------------------------------------------------------------------

## 14. Recipes (longer)

### Centred title slide with accent bar

``` css
.title-slide {
  background: white;
  text-align: center;
  display: flex; flex-direction: column; justify-content: center;
}
.title-slide h1 {
  font-size: 60px; font-weight: 700; color: #17324d;
  max-width: 900px; margin: 0 auto 0.15em auto;
  border-bottom: 4px solid #2E6E9E;
  padding-bottom: 0.2em; display: inline-block;
}
.title-slide h2 { font-size: 26px; font-weight: 400; color: #555; margin-bottom: 1.6em; }
.title-slide h3 { font-size: 20px; font-weight: 400; color: #333; }
```

### Persistent venue footer + small slide-number

``` css
.remark-slide-content::after {
  content: "ANU Biometrics Seminar · 2026";
  position: absolute; bottom: 16px; left: 64px;
  font-size: 14px; color: #94a3b8;
}
.remark-slide-number { font-size: 14px; color: #94a3b8; bottom: 14px; right: 18px; }
```

The `::after` pseudo-element approach injects the footer text on every slide without touching the Rmd.

### Equal-height two-column with figure on right

``` css
.cols-fig { display: grid; grid-template-columns: 1.1fr 1fr; gap: 30px; align-items: stretch; }
.cols-fig img { max-height: 460px; object-fit: contain; align-self: center; }
```

``` markdown
.cols-fig[
.col-a[
- bullet one
- bullet two
]
.col-b[
![](figures/foo.png)
]
]
```

### Highlight a key bullet

``` css
.remark-slide-content li.key {
  background: #fff6cc; padding: 2px 6px; border-radius: 3px;
}
```

In Rmd, remark doesn't directly let you class a single `<li>`. Wrap instead:

``` markdown
.key[**This is the key bullet**]
```

with `.key { background: #fff6cc; padding: 2px 6px; border-radius: 3px; }`.

------------------------------------------------------------------------

## 15. Browser-inspector workflow

1.  Render the deck (`rmarkdown::render("divas-anu.Rmd")` or use Infinite Moon Reader).
2.  Open `divas-anu.html` in Chrome / Firefox.
3.  F12 → click the element you want to tweak.
4.  In the Elements pane you see every class applied; in the Styles pane you see every CSS rule and which file it came from.
5.  Edit values live in the Styles pane to preview; once happy, write the rule to `custom.css`.

Strikethrough in the Styles pane means a later rule overrode that property — useful for tracking down "why doesn't my colour change apply".

------------------------------------------------------------------------

## 16. Common gotchas

- **`max-width` without `margin: 0 auto`** → element is shrunk but not centred.
- **`<br/>` inside a YAML `title:`** renders as a literal break inside the `<h1>` and forces a tall multiline heading. Split into `title:` + `subtitle:`.
- **`!important` battles with MathJax.** MathJax injects inline styles; you often need `!important` on `.MathJax` rules.
- **`.title-slide-credits`** (old xaringan idiom) is not standard — only works if you also wrote the corresponding HTML. Prefer `.title-slide h3` and YAML `author:` / `date:`.
- **Custom classes added in `class:` lines (e.g. `class: inverse`) must be defined in `custom.css`** or they silently do nothing.
- **`.pull-left` heights** — both columns float; if the left column is much taller, the next paragraph slides under the right column. Use the Grid recipe if you hit this.
- **Knitr fig sizes** are independent of CSS — `out.width` controls the rendered HTML width, but the underlying PNG resolution is set by \`fig.width
  - dpi\`. Mismatched values give blurry figures.

------------------------------------------------------------------------

## 17. External references

- [Yihui Xie's R Markdown book — Ch. 7 xaringan](https://bookdown.org/yihui/rmarkdown/xaringan.html) — official walkthrough.
- [xaringan repo](https://github.com/yihui/xaringan) — read `inst/rmarkdown/templates/xaringan/resources/default.css` once.
- [xaringan wiki](https://github.com/yihui/xaringan/wiki) — recipe FAQ.
- [xaringan demo](https://slides.yihui.org/xaringan/) — every feature, live.
- [remark wiki](https://github.com/gnab/remark/wiki) — class syntax + slide properties (the layer beneath xaringan).
- [xaringanthemer](https://pkg.garrickadenbuie.com/xaringanthemer/) — R-controlled theming if you'd rather not hand-roll CSS.
- [xaringanExtra](https://pkg.garrickadenbuie.com/xaringanExtra/) — plugins (tile view, panelset, scribble, share-again).
- [gadenbuie/xaringan-themes](https://github.com/gadenbuie/xaringan-themes) — full example `custom.css` files (kunoichi, ninjutsu, lucy, etc.).
