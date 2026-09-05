# Finding the .Self

Source for [findingtheself.app](https://findingtheself.app) (this folder) and
[journal.findingtheself.app](https://journal.findingtheself.app) (`/journal`).

**New teacher added, 5 Sept 2026: Nisargadatta Maharaj.** `index.html` now has an eighth teacher
article (`id="nisargadatta"`), placed right after Ramana Maharshi in the Eight Voices section —
bio + four short quotes from *I Am That*, matching the existing teacher-bio format exactly. All
"seven teachers / seven voices" copy on the page (meta description, hero intro, About-Sandeep
paragraph, section heading, Daily Inspiration blurb) was updated to "eight" to match. The journal
(`journal/index.html`) also picked up three new self-inquiry `WRITE_PROMPTS` drawn from his core
teaching (abiding in the plain sense "I am," before any story about being someone gets added).
Nothing changed in the meditation app — a full "I Am" guided-session script was discussed but not
built yet; ask if that's still wanted.

**Follow-up fix, same day:** the "Inspiration ▾" nav dropdown (`#inspiration-menu` in `index.html`)
has its own separate, hardcoded list of teacher links — adding the new `<article>` section didn't
touch it, so it still only showed seven. Added `<a href="#nisargadatta">Nisargadatta</a>` there too
(right after Maharshi, same placement logic), and caught one more stray "seven voices" reference in
`self-system.html`'s blog product-card copy. Checked the rest of the repo for any other hardcoded
teacher lists or counts — none found; this dropdown was the only other spot.

**The "Self and the Machine" reflection is now a real page, same day:** it had only been delivered
as plain text before. Added `reflections/day-nine-the-self-and-the-machine.html` (same markup as
the other eight day-entries — rail header, entry-meta, body, related-teaching box cross-linked to
the new Nisargadatta section, prev/next nav), wired Day Eight's "next" link forward to it, added it
to the top of `reflections/index.html`'s list, and swapped it into the homepage's 3-entry teaser
(bumping Day Four out of the teaser — still reachable from the full Reflections index, nothing
deleted). Verified all three pages render cleanly with Playwright before zipping.

**New page, not yet linked from anywhere (4 Sept 2026):** `the-self-and-ai.html` — an essay ("The
Self and the Machine") reworking the "Ultimate Self & AI" comparison artifact into the site's own
voice and design system (same tokens/typography as `self-system.html`, prose instead of an
icon-and-stat-card infographic). It's a standalone file right now — nothing in the nav or homepage
links to it yet, on purpose, since where it should live (main nav? a homepage teaser? folded into
Reflections?) hasn't been decided.

## How this repo came to exist

This code was rebuilt by Claude on 2 Sept 2026, directly from the **live, deployed site** — not
copied from an original source file. The site was originally built as a Claude artifact, but this
session couldn't read that artifact's source (a network restriction on the session, since fixed),
so instead everything here was reconstructed from what's actually live: the real page content, and
the site's own CSS custom properties (colour/type tokens), read with browser automation.

That means:
- The **content is real** — copy, quotes, structure all pulled from the live pages, not invented.
- A handful of paragraphs (marked `<!-- TODO -->` in `index.html`) were cut off mid-sentence when
  read and needed to be completed by copying the rest from the live site. (The 6 Reflections entries
  had this same issue and have since been fixed — see below. **The My Master paragraph is now fixed
  too** — Sandeep sent the missing sentence and the full Guru Gita verse + translation on 3 Sept
  2026, replacing the truncated version. **Update, 4 Sept 2026: all remaining truncated teacher
  content is now fixed too** — pulled straight from the live site with WebFetch. That covers the
  Bodhidharma and D.T. Suzuki bios (both were still marked `TODO`), the Nāgārjuna and Osho bios
  (found truncated while double-checking, no `TODO` marker on them), and five blockquotes that had
  been cut off ending in "…" — the Ramana Maharshi, Suzuki (×2), Nāgārjuna, and Osho quotes. There
  are no more `<!-- TODO -->` content gaps left in `index.html`'s teacher/quote content. The three
  Reflections excerpts on the homepage still end in "…" by design — they're teaser text linking to
  the full post, not truncation bugs.)
- **Update: the two missing photos are in.** `about-sandeep.jpg` (the Barong mask ceremony selfie)
  and `about-master.jpg` (with Maitreya Prema at the temple entrance) were added on 3 Sept 2026 —
  Sandeep sent the originals, they've been auto-rotated (their EXIF orientation was sideways) and
  downsized for web (max 1400px wide, ~85% JPEG quality) and saved under those exact filenames at
  the repo root, matching what `index.html` already expected.
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
   **Reflections has now been split out** (see below) — the rest (About, My Master, the 7 teacher
   profiles) is still one long scroll.
2. The nav has no mobile menu — on a narrow screen it can silently overflow. This reconstruction
   makes the nav wrap instead of overflow-scroll, as a safer default, but a real mobile nav pattern
   (hamburger menu) is still worth building.

## Reflections now have real per-entry URLs

`reflections/` is a new folder: `reflections/index.html` lists all 6 entries, and each entry has
its own page (e.g. `reflections/day-one-just-sitting.html`) with its own URL, meta description,
and a "related teaching" cross-link back to whichever teacher's page fits that entry's theme. The
homepage's Reflections section is now a short teaser (latest 3) linking to `reflections/index.html`
instead of dumping all 6 entries inline. This is what makes the blog actually shareable and
indexable — each entry can now be linked to directly, and searched for directly, instead of only
existing as a scroll-position on the homepage.

**All 6 entry bodies are now complete** — the `<!-- TODO -->` placeholders that were cut off
mid-sentence have been replaced with the real text, pulled directly from the live site's
`#reflections` section (not rewritten). One small copyedit: Day Eight's text had a couple of minor
grammar slips on the live site ("When you think about there is little...", a missing "it"); those
were smoothed out here without changing anything he said. If that wasn't wanted, the original
phrasing is still live at findingtheself.app to restore from.

## The .Self system (new)

`self-system.html` (+ `self-system.css`) is a new concept page introducing the unifying brand
across all three products: **Finding the .Self** (this blog), **Witness.Self** (the meditation app,
not built yet — the card links nowhere on purpose and says so), and **Reflect.Self** (the journal,
linking to `journal/index.html`). It explains the shared ethos — witnessing/recognition, grounded
in the Kashmir Shaivism (Trika) tradition specifically (pratyabhijñā, spanda, vimarśa) rather than
generic "mindfulness" — and is linked from the main nav as **.Self**. Same design tokens and type
system as the rest of the site; no new patterns invented. This is a first-pass concept page, not a
rebuild of the meditation app or journal themselves — those are separate, bigger jobs.

**Update: the blog's own name now carries the mark too.** Every place the site displayed "Finding
the Self" — the homepage `<title>`, the sticky-nav wordmark, the homepage hero `<h1>`, the other
pages' `<title>` tags, and this README — now reads **Finding the .Self**, so all three products
share one wordmark family (`Witness.Self`, `Reflect.Self`, `Finding the .Self`). The nav and hero
instances use the same seal-colored `.dot` span the journal and Witness.Self already use for their
own wordmarks (`.dot { color: var(--seal); }`, added to `index.html`'s inline styles, `self-system
.css`, and `reflections/reflections.css`); references to the blog's name inside `self-system.html`'s
body copy and card, and in this README, stay plain text to match how the other two products are
already referenced there. The domain itself — `findingtheself.app` — is unchanged; this is a display
wordmark change only.

## Homepages now open with the manifesto statement from the pitch decks

At the user's request to take the deck's look "further" onto the real pages, the blog homepage
(`index.html`) and `self-system.html` now open with the same layout as the ethos deck's cover
slide: a small mono eyebrow, a bold two-line declarative statement, then the lockup mark — instead
of leading with the wordmark alone. `index.html`'s hero now opens with **"Not a wellness brand. A
way of paying it forward."** (the line from the ethos deck) under the eyebrow "Why this exists,"
with the full vertical lockup graphic (a larger version of the header mark below) right after it;
all the existing explanatory paragraphs stay, unchanged, right below. `self-system.html` keeps its
existing "One house. Three rooms." headline and adds the same lockup graphic beneath it. Both
lockups use `role="img"` with a proper `aria-label` rather than exposing the three-line text
straight to screen readers, since read literally in DOM order ("Finding the… Witness the… Reflect
the… .Self") it doesn't parse as a sentence.

Witness.Self's welcome screen got a lighter version of the same touch: a small eyebrow line
("Recognition, not relaxation") above the "Witness" heading. While in there, fixed a real content
bug found along the way — the welcome description said "guided by four voices" and listed only
four teachers, when there are five (Kali was missing from the sentence, though always present in
the actual practice). Also fixed a real **layout bug**, unrelated to this pass but caught while
checking a full-page screenshot: `footer.app-footer` was closing outside `.app`'s wrapping `<div>`,
which made it a flex sibling of `.app` under `body`'s row-flex layout — so it rendered pinned to
the top-right of the page instead of at the bottom. Moved the footer back inside `.app`, after the
last screen section; it now sits correctly below the visible screen, as originally intended.

Reflect.Self (`journal/index.html`) was deliberately left alone beyond the header mark added
earlier — its screens are already tight, one-job-per-screen, and adding a big manifesto statement
there would work against the "uncluttered sanctuary" standard the rest of the app already meets
well; its own short intro lines ("Notice what's here, then put it into words.", "you're not the
weather. you're the sky.") already carry the same voice.

## The three products now carry a shared "lockup" mark in their headers

A small graphic — **Finding · Witness · Reflect** converging on a short line into **.Self** — now
appears in the header of all three products: the blog's main nav (replacing the plain `.Self` text
link), the journal's header (replacing the "part of finding the self" subline), and Witness.Self's
header (same). It's a compact, single-line adaptation of a mark built for a pitch deck, sized to fit
existing header heights without adding a mobile-nav problem on top of the one already flagged in the
audit. Each one links to `self-system.html`. New CSS class: `.self-lockup` (+ `.lk-names` / `.lk-mark`
inside it), defined locally in each file rather than shared, since the three headers don't share a
stylesheet.

## The journal, renamed and refined to Reflect.Self

`journal/index.html` is now branded as **Reflect.Self** in the UI (wordmark, tab, and a footer
line back to `self-system.html`), and its copy was refined throughout so it reads as a witnessing
practice rather than generic self-improvement tracking — the cloud header, the Write/Inquiry/
Patterns/Traits intro lines, and one new prompt all now name noticing/watching directly instead of
implying it. No structural or data changes: same four screens, same `TRAITS`/`WRITE_PROMPTS`/
`INQUIRY_QUESTIONS` arrays (one prompt added), same `TODO: Supabase` markers, same localStorage
fallback. Streak language softened from "day streak" to "days of watching" to match the ethos — and then,
on a follow-up pass, the mechanic itself changed, not just the wording: it's no longer a
consecutive-day streak (which resets to zero and creates the "don't break the chain" pressure
streaks are designed around). It's now a plain count of distinct days you've actually written an
entry or saved a self-inquiry reflection, derived from the real data each time rather than a
separately incremented counter. Missing a day costs nothing — there's no chain to break, just a
number that goes up when you show up.

## Witness.Self is now real (not a concept card)

`witness/index.html` is a full, working sitting-practice app — recovered from an existing Claude
artifact called "Witness" (built the day before this repo's rebuild, previously unopened) rather
than built from scratch. It was already far more complete than a first pass would have been: a
welcome screen, a setup screen (duration, posture — sitting/walking/standing/lying, and a choice of
five witnessing teachers), a session screen with a breathing ensō animation and a countdown that
**stays hidden by default and only reveals itself on tap** (a watched clock works against the whole
point of sitting — that restraint was already built in), and a completion screen. Guidance is
spoken aloud via pre-recorded audio clips with a live speech-synthesis fallback, and a soft ambient
layer (bell, cloud-clearing tone, Om chant, high tones) is synthesized live with the Web Audio API —
no external audio files, no backend, nothing to wire up. All five techniques are grounded in real
teachers: Osho (witnessing), Nāgārjuna (emptiness), Bodhidharma (wall-gazing), Ashtavakra (I am the
witness), and Kali (energy and space) — verbatim voice-over scripts already written for each, with
posture-aware variants.

The only change made this pass was branding: the header now reads **Witness.Self** with a "part of
finding the self" subline (matching Reflect.Self's wordmark treatment), and a footer link back to
`self-system.html` naming *pratyabhijñā*. No functional code was touched. `self-system.html`'s
product card now links to it instead of showing "in progress."

**Update: Witness.Self now counts days too.** Same mechanic as Reflect.Self, on purpose — a plain,
non-resetting count of the distinct calendar days you've sat, stored as a short list of completion
timestamps in `localStorage` (`witness-sessions-log`) and reduced to a distinct-day count the same
way `daysWatchedCount()` does in the journal. No minimum length to "qualify" a sit, and ending early
still counts — showing up is the point, not finishing. It shows as "**N** days you've sat, in total"
on the completion screen, and quietly on the welcome screen too once it's above zero (hidden for
first-time visitors, since a "0 days" line has nothing to say). Sitting twice in the same day still
only counts as one day — same as journaling and self-inquiry both counting as one day of watching in
Reflect.Self. Smoke-tested end to end with Playwright (singular/plural text, persistence across a
reload, same-day sits not double-counting).

## Getting this into GitHub (you're new to this — here's the short version)

1. On [github.com](https://github.com), click **New repository**. Name it (e.g. `findingtheself-app`), leave it empty (no README/license), and create it.
2. On the new repo's page, click **uploading an existing file**.
3. Drag in everything from this folder (`index.html`, the `journal/` folder, this `README.md`) and commit.
4. Add the two missing images (see above) the same way, or later.

Once it's in GitHub, you can connect it to Netlify (**Add new site → Import an existing project →
Deploy with GitHub**, pick this repo) so future pushes to GitHub deploy automatically — replacing
however the site is deployed today.

## Next steps, in order

1. All content gaps are filled in now — the two images, the My Master paragraph, and every
   truncated teacher bio/quote are done. Nothing left marked `<!-- TODO -->` for content.
2. Wire up Supabase in `journal/index.html` (or share your project URL/anon key and Claude can do it).
3. Come back to the audit findings and decide what to actually change — the homepage split and the
   mobile nav are the two worth doing first.
