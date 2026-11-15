# Preetam Priyabrat, portfolio

Single-file site. Deploy this folder to Netlify (drag onto app.netlify.com/drop) or GitHub Pages as-is.
No build step and no external requests: fonts, logos and images all ship inside the folder.

## Page order

Hero · Why am I the right fit · Tools I work in · Brands I have pulled apart · Seeing the bigger
picture (carousel) · Projects, teardowns and strategies (grid or list) · How I think about growth ·
A brand I started myself · Where I have worked · Tell me the role · Where I studied · Writing · Contact

Every section leads with its heading; the small label underneath is secondary by design.

## Editing content

Everything editable lives in one block at the top of the `<script>`.

- `PILLARS` - the seven strands of work: Marketing, Category, E-com & Q-com, Growth, Strategy,
  Demand Gen and Product Marketing. They drive the rotating hero line, the index filters and the
  role picker. Change them here and everything downstream follows, so a new pillar needs
  `ROLE_GROUPS` entries and at least one `WORK` item tagged with it, or its filter comes back
  empty. There is no separate list of disciplines anywhere: the rotator is the only place they are
  spelled out, which is deliberate. The B2B sales roles still exist in `ROLEDATA`; they live under
  Demand Gen (Account Executive, Enterprise and Government Sales) and Strategy (Business
  Development, Partnerships), because a pillar with one project behind it reads as a gap.
- `WORK` - every piece of independent work. `tags` must be pillar names. `status`: `"live"` (needs
  `url`), `"doc"` (has `docs`), `"soon"` (work done, write-up not published yet, shows an amber
  pill and a footer chip and dims the card arrow), `"progress"` or `"done"`. `featured:true` puts it in the carousel.
  `image` is the card picture, `shot` is the browser-frame screenshot in the carousel, `tone` tints
  the card tile.
- `shot` - a real screenshot of the live page, shown in the carousel browser frame. Capture the
  first screen and save it as `assets/work/<id>-shot.jpg` at exactly **1280x682**, which is what
  `.bw-shot`'s aspect-ratio expects. Scale and crop to reach that size, never resize to a different
  ratio: that stretches the picture, and it is the one mistake this frame will not hide. If you
  change the capture size, change the `.bw-shot` aspect-ratio with it. `image` stays the card thumbnail in the grid, which is a different
  picture on purpose.
- `cover` - a first-page render of the document, shown in the carousel. Make one with
  `pymupdf` at 150dpi and drop it in `assets/covers/<id>.jpg`.
- `docs` - downloadable files on a card: `[{label, file, kind}]`. The whole card downloads the first
  one; each file also gets its own chip, which is how NYOD offers the diagnosis and the forecast
  model separately. Files live in `assets/docs/`.
- `LOGOS` - brand marks in `assets/logos/`. `inv:true` flips a white mark to black on light tiles.
- `TOOL_ICONS` / `TOOLS` - the stack. A tool with no icon entry falls back to a lettermark.
- `ROLE_GROUPS` and `ROLEDATA` - the role picker, grouped by pillar. Pick a pillar, then a role.
- `FUNNEL`, `CAPS`, `GRIPPLY_PHOTOS`, `LADDER` - the growth loop, the capability overview and the
  Gripply gallery.
- Work experience, the hero bubbles and the numbers strip are plain HTML in the body, not data.

## The hero

The photo is `assets/img/hero.webp`, the original wide cut-out. It runs edge to edge with the desk
sitting on the bottom edge of the section, and the whole hero is sized to one screen (`100svh`).
The four stat bubbles sit top-right on desktop and drop under the buttons on phones.

Art height is `--ah` on `.hero3` (`clamp(300px, max(27vw, 61vh), 820px)` on desktop). The image is
positioned with `left: min(0px, 82vw - 3.278 * var(--ah))`. The photo is 5160x941 and his head sits
at 59.8% of its width, so `3.278 = 0.598 x 5.483`; the `min(0px, …)` keeps the desk spanning the
full viewport on wide screens. Phones use a smaller `--ah` and `58vw`, and the whole hero still
fits one screen (822px tall at 390x844).

## The headline

The `<h1>` is three `.ln` spans, each one line: "I drive", the rotator, "for scaling businesses."
The breaks are hand-set rather than left to wrapping, because at this size an organic wrap leaves
a two-word orphan. If you change the wording, check all three breakpoints (1440, 1100, 390) and
keep every line to one line; `.hero3-main` is capped at `min(560px, 46vw)` so the rotator's
longest word is what sets the ceiling. Each line has its own transition delay, so a fourth line
needs a fourth `nth-child` rule.

The lede under it is capped at `min(100%, 40ch)` and the ROAS bubble moves to the right edge
between 861px and 1120px. Both exist for the same reason: the bubbles are positioned as a
percentage of the hero, so on a short window they ride up into the text. If you widen the lede or
move a bubble, re-check a 700px-tall viewport, which is where it breaks first.

## Dark mode

Dark is the base theme; the toggle switches to light. The sun/moon button sits in the nav beside Get in touch, LinkedIn and
Gmail. `<html>` ships with `data-theme="dark"`; the toggle removes it for light and saves the
choice in `localStorage` under `pp-theme`, and a tiny inline script at the top of `<body>`
reapplies a saved light preference before first paint, so there is no flash. All colour lives in custom properties redefined under `html[data-theme="dark"]`, plus
explicit overrides for the places v1 hardcoded white. Brand marks in the showcase keep their own colour
in both themes. A mark that is solid black is tagged `dk:true` in `LOGOS` and turned white in
dark; one that is already white is tagged `inv:true` and turned black in light; one that mixes a
coloured symbol with black type, or whose single colour is too dark to read on black, ships a
second file via `srcDark` (JioMart, Kuku, Wakefit). Logo tiles sit on a light tint in both themes,
so they always use the light mark. The browser mockups
in the carousel stay a light window in both themes, the way a real browser does.

## Project order

`WORK` order drives both the carousel and the grid. It is hand-ordered, strongest first:
District, Phab, Sasta Kaun, Snitch, Kuku, Wakefit, Brewcontent, Zoomies, Wondergifts, Sova, then
the rest. Reordering the array reorders both views; nothing else needs touching.

## The carousel

The card is about 500px tall so one fits a laptop screen without scrolling; the compaction rules
live in the "CAROUSEL: one card, one screen" block and only apply above 861px. Under the track is
a dot strip, one dash per project, built from the cards themselves, so it needs no maintenance
when a project is added. Arrows still work and the active dash widens.

Every project appears in it. What shows in the frame depends on what the project is: a browser
window with a real screenshot of the live page for a web study, a document window with the PDF's
own first page for a report, and a flat thumbnail for everything else. The caption under each one
is the piece's own headline.

Card copy is written as a statement of what the work is, never as a problem framed back at the
reader: `q` is the headline, `a` is what was actually done. Keep it that way when adding projects.

## The growth funnel

The five stage bars are tabs. Each carries a chevron that nudges sideways on a loop until you
hover, rotates down when selected, and sits inside a ring on the unselected ones, with a hint line
above the stack. If you restyle them, keep an affordance: without one nobody discovers the panel
changes.

## Galleries

Gripply renders one flat grid, three columns on desktop, with every tile locked
to 4:3 so the rows stay flush. It interleaves three info tiles (brand line, pop-up stats,
price ladder) among the photos; they share the same aspect, so their content is sized to fit
rather than the grid flexing around them. One portrait photo, the launch poster, is listed in
`GRIPPLY_WIDE` and takes a two-row cell so it is not cropped; the layout order is arranged so the
rows still come out even. Captions are one descriptive line, clamped to three.

## The index

Cards are a grid by default, with a Grid / List toggle beside the filters. The whole card is one
link: it opens `url` in a new tab, or shows a "link coming soon" toast when `url` is empty.

## Adding a project screenshot

Open the live study at about 1180px wide, screenshot the first screen, save it as
`assets/work/<id>-shot.jpg` at 1280x682, and set `shot` on the `WORK` entry (`image` is the
separate grid thumbnail). This container cannot reach Netlify, so the capture has to come through
a browser on the Mac. The built-in browser pane returns its screenshots at the pane's own size
rather than the emulated viewport, so a shot taken that way is scaled up to 1280x682; the
carousel frame renders it about 400px wide, so the upscale does not show.
District, Phab and Sasta Kaun already have theirs.

## Still to add

- Live URLs for the four projects still marked "Write-up coming soon" (Bombay Shaving Company,
  Emami Agrotech, CheQ, Pocket FM).
- **A clean Bombay Shaving Company logo.** The current `assets/logos/bsc.png` is cropped in the
  source file, so "COMPANY" is cut off wherever it appears. Replace the file and it fixes itself.
- Wakefit logo in `assets/logos/`, then add it to `LOGOS`.
- Shakti Transformers dates in the experience section.
- Sixteen tools have no public vector logo (GeM, Dynamics 365, Clarity, Flipkart Seller Hub, the
  q-commerce seller portals, CleverTap, MoEngage, WebEngage, Klaviyo, AppsFlyer, Ahrefs, Sprout
  Social, Shiprocket, Unicommerce, Google Trends, Merchant Center). They use brand-coloured
  lettermark tiles in `assets/tools/`. Drop a real SVG or PNG over the file to replace one.

## Files

- `index.html` - the whole site.
- `writing/` - three long-form articles, linked from the Writing section. They share
  `assets/site.css`, which is the same stylesheet the index carries inline, so the design system
  and the dark-mode toggle stay in step. Edit `add.css` and rebuild both.
- `assets/site.css` - the shared stylesheet for the article pages only.
- `index-v1-backup.html` - the original version, kept in case anything needs rescuing.
- `assets/img/hero.webp` - the hero photo. `hero-cut.webp` is a cropped version, now unused.
- `assets/work/` - study screenshots.
- `assets/logos/linkedin.png`, `assets/logos/gmail.png` - the nav and contact icon buttons.
- `assets/docs/` - the downloadable PDFs and the NYOD Excel model.
- `assets/covers/` - first-page renders of each PDF, used as the carousel preview.
- `assets/tools/` - tool marks for the stack.
