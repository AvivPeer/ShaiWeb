# CLAUDE.md

Context for Claude Code sessions on this repo. Read this before touching anything.

---

## What this is

A single-page Hebrew marketing website for a private physiotherapy clinic in
Nahariya, Israel. The practitioner is Shai Peer, a certified physiotherapist
(B.P.T). The site is being built by his brother.

**The page has exactly one job:** take a person who is in pain and searching for
help, build enough trust in about forty seconds, and get them to send a WhatsApp
message or call. Every design and copy decision serves that. If a proposed change
does not help someone in pain make contact, it is probably not worth making.

Audience: Hebrew-speaking adults in the Western Galilee, mostly on phones, often
in some discomfort while reading. Assume small screens and short patience.

---

## Stack — and the reasoning, because it looks unusual

Plain HTML, hand-written CSS, and vanilla JavaScript. All of it in one
`index.html`. No framework, no build step, no package.json, no runtime CDN
dependency.

This is deliberate, not laziness:

1. **A CDN dependency already broke this project once.** The first version used
   the Tailwind Play CDN. It failed to load in a sandboxed preview and the entire
   page rendered as unstyled HTML. A clinic site that depends on a third-party
   script to be *legible* is fragile in a way that is not worth the convenience.
2. **The owner is not a developer.** He needs to change a phone number or a
   sentence without running a build.
3. **Speed.** One file, no JS framework, no hydration. On a phone on cellular,
   this matters more than any feature.
4. **Vercel serves it as pure static output.** Nothing to configure.

**Do not introduce React, Next.js, Tailwind, or a bundler without asking.** If
the project grows to multiple article pages, the migration conversation is worth
having — see `docs/ROADMAP.md` — but it is a decision for the owner, not a
default.

The only external resource is the Google Fonts stylesheet, and it has a full
fallback stack behind it (`Frank Ruehl CLM` → `David` → `Georgia`). If the fonts
fail, the layout still holds.

---

## Hebrew RTL rules — non-negotiable

These are the things that separate a Hebrew site that feels professional from one
that feels machine-translated. Preserve them in every edit.

| Rule | Why |
|---|---|
| `letter-spacing: 0` on all Hebrew text | Hebrew letters are boxy and share a visual baseline. Tracking them apart destroys word shapes and legibility. Never track out Hebrew display type, no matter how elegant it looks in Latin. |
| Wrap every Latin word and number run in `<span class="ltr">` | `.ltr` applies `unicode-bidi: isolate`. Without it browsers reorder surrounding punctuation and you get `.B.P.T` or reversed time ranges. This is the single most common Hebrew web bug. Applies to: `B.P.T`, `TMJ`, `BPPV`, `WhatsApp`, `Evidence-Based`, `Dry Needling`, phone numbers, times, `45–60`. |
| `line-height: 1.75` on body text | Hebrew has no ascenders or descenders, so text blocks read denser than Latin at identical leading. |
| Hierarchy from weight, size, and color only | Hebrew has no uppercase. Any hierarchy trick that relies on caps does not exist here, so the type scale carries more load than it would in English. |
| Geresh `׳` and gershayim `״`, never ASCII `'` or `"` | Correct Hebrew punctuation. Used in `דק׳`, `א׳–ה׳`, `דוא״ל`, and quotation marks. |
| `<html lang="he" dir="rtl">` | Never change. |
| Use logical CSS properties | `inset-inline-start`, `margin-inline`, `padding-inline` — not `left`/`right`. The layout mirrors correctly and stays maintainable. |

---

## Design system

Tokens live in the `:root` block at the top of `index.html`. Read them there;
do not hardcode values elsewhere.

```
--ink        #0A1F33   headings, primary buttons, dark sections
--ink-2      #16344F   body text in emphasis contexts
--slate      #5A7183   secondary and muted text
--aqua       #16A39B   accent, CTAs, active states
--aqua-deep  #0B7C77   accent hover
--porcelain  #F4F7F8   tinted section backgrounds
--line       #E2EAEE   hairline borders
```

Type: **Frank Ruhl Libre** (Hebrew serif, weights 500/700/900) for display and
headings; **Assistant** for body and UI. This pairing is a deliberate departure
from the Rubik/Heebo default that most Israeli clinic sites use — the serif reads
as an experienced practitioner rather than a template.

Sizes use `clamp()` for fluid scaling. There are no breakpoint-specific font
sizes; do not add any.

**Restraint is the design.** Boldness is spent in exactly one place — the body
map. Everything around it stays quiet: hairline rules, one accent color,
generous whitespace. If you add a second attention-grabbing element, the first
one stops working.

---

## The signature element

The interactive body map in the hero is the thing this site is remembered by.
Tapping an anatomical point swaps the adjacent panel and rewrites the WhatsApp
link with a pre-composed message ("היי שי, יש לי כאבי צוואר...").

It exists because it removes the hardest step for someone in pain: knowing what
to write in the first message. Keep it. It is keyboard-accessible (`tabindex`,
`role="button"`, `aria-pressed`, Enter/Space handlers) — keep that too.

Region data lives in the `regions` object in the script at the bottom of
`index.html`. Adding a treatment area means adding an entry there plus either an
SVG hotspot or a chip.

---

## File map

```
index.html      Everything. Structure: tokens → reset → typography → layout →
                components (header, hero, map, about, areas, process, quotes,
                faq, contact, footer) → motion. Then the HTML, then JSON-LD,
                then the script. Section comments are numbered — follow them.
robots.txt
sitemap.xml     Update when article pages are added.
vercel.json     Security headers, asset caching, clean URLs.
docs/DESIGN.md  Extended design and typography notes.
docs/CONTENT.md Placeholders and the checklist of what is still needed from Shai.
docs/ROADMAP.md SEO plan and phase-two features.
```

---

## Quality floor — verify before any commit

- Renders correctly at 360px width. Test this first, not last.
- Keyboard navigable end to end, with visible focus rings.
- `prefers-reduced-motion` disables all animation.
- No horizontal scroll at any width.
- All Latin and numeric runs bidi-isolated.
- JSON-LD still validates (Google Rich Results Test).
- No placeholder values shipped to production — see below.

---

## Blockers — do not deploy to a real domain with these in place

`index.html` still contains placeholders. They are listed in `docs/CONTENT.md`
with exact search-and-replace strings. The phone number `050-000-0000` and the
address `הרצל 1, נהריה` are invented. Shipping them would send real patients to
a dead number.

---

## Content policy

This is medical marketing, which carries real obligations:

- **No efficacy guarantees.** No "we will cure your pain", no recovery
  timelines presented as promises. Existing copy is careful about this; keep it
  that way.
- **Testimonials must be real and approved by the patient.** The two currently in
  the file came from the owner's draft and still need confirmation.
- **Keep the footer disclaimer** ("האתר אינו מהווה ייעוץ רפואי או תחליף לבדיקה").
- Describe what happens in treatment plainly. Do not oversell.

---

## Running it

No build. Open `index.html` in a browser, or:

```bash
python3 -m http.server 8000     # then visit localhost:8000
npx vercel dev                  # if the Vercel CLI is installed
```

---

## Status

Design and build complete for the one-page version. Awaiting real contact
details, a portrait photo, and content confirmation from Shai. Not yet deployed.

Next up, in order: fill in real content → deploy to Vercel → Google Business
Profile → article pages for headaches, TMJ, and vertigo.
