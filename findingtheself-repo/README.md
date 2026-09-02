# Finding the Self

Source for [findingtheself.app](https://findingtheself.app) (this folder) and
[journal.findingtheself.app](https://journal.findingtheself.app) (`/journal`).

## How this repo came to exist

This code was rebuilt by Claude on 2 Sept 2026, directly from the **live, deployed site** — not
copied from an original source file. The site was originally built as a Claude artifact, but this
session couldn't read that artifact's source (a network restriction on the session, since fixed),
so instead everything here was reconstructed from what's actually live: the real page content, and
the site's own CSS custom properties (colour/type tokens), read with browser automation.

That means:
- The **content is real** — copy, quotes, structure all pulled from the live pages, not invented.
- A handful of paragraphs (marked `<!-- TODO -->` in `index.html`) were cut off mid-sentence when
  read and need to be completed by copying the rest from the live site.
- **Two images are missing**: `about-sandeep.jpg` and `about-master.jpg`. Save them from
  `findingtheself.app/about-sandeep.jpg` and `findingtheself.app/about-master.jpg` and drop them in
  this folder (same filenames) before deploying.
- **The journal's backend isn't wired up.** The live journal uses Supabase (it loads
  `@supabase/supabase-js`) to save entries, track your streak, and store traits. This
  reconstruction uses the browser's local storage instead, just so the shell works out of the box —
  see the `TODO: Supabase` comments in `journal/index.html` for where to swap in your real project
  once you have its URL and anon key.

None of this is a redesign — it's meant to match what's live today, so you have a safe, real
starting point in version control. Improvements (see below) are a separate, deliberate next step.

## Design tokens

Colours, type, and component patterns are documented in more depth in the `Findingtheself.app`
Claude project (`claude/design-system.md`), but the short version — these are the actual CSS custom
properties used in both HTML files:

| Token | Light | Dark | Use |
|---|---|---|---|
| `--paper` | `#EDE6D6` | `#1A1712` | Background |
| `--paper-raised` | `#E3D9C2` | `#221D16` | Cards |
| `--ink` | `#221F1A` | `#ECE3D0` | Primary text |
| `--ink-soft` | `#57503F` | `#C2B79C` | Secondary text |
| `--seal` | `#A8362A` | `#D9836A` | Accent — links, buttons |

Type: **Cormorant Garamond** (headings), **Newsreader** (body), **IBM Plex Mono** (uppercase UI
labels/buttons/nav). Buttons are 4px radius, not soft-rounded.

One deliberate change from the live site: `--ink-faint` has been corrected here (`#6B6355` /
`#93896E` instead of the live `#8C8271` / `#83795F`), because the live value fails WCAG AA contrast
— see `claude/ui-audit-2026-09-02.md` in the Claude project for the full writeup. If you want to
match the live site exactly instead, swap it back.

## Known issues carried over from the live site (see the full audit for detail)

1. The homepage is one very long single-page scroll doing several jobs at once (mission, memoir,
   tribute, 7 teacher bios, blog, app promo, email capture). Worth splitting into real pages.
2. The nav has no mobile menu — on a narrow screen it can silently overflow. This reconstruction
   makes the nav wrap instead of overflow-scroll, as a safer default, but a real mobile nav pattern
   (hamburger menu) is still worth building.

## Getting this into GitHub (you're new to this — here's the short version)

1. On [github.com](https://github.com), click **New repository**. Name it (e.g. `findingtheself-app`), leave it empty (no README/license), and create it.
2. On the new repo's page, click **uploading an existing file**.
3. Drag in everything from this folder (`index.html`, the `journal/` folder, this `README.md`) and commit.
4. Add the two missing images (see above) the same way, or later.

Once it's in GitHub, you can connect it to Netlify (**Add new site → Import an existing project →
Deploy with GitHub**, pick this repo) so future pushes to GitHub deploy automatically — replacing
however the site is deployed today.

## Next steps, in order

1. Fill in the `TODO` content gaps above (truncated paragraphs, two images).
2. Wire up Supabase in `journal/index.html` (or share your project URL/anon key and Claude can do it).
3. Come back to the audit findings and decide what to actually change — the homepage split and the
   mobile nav are the two worth doing first.
