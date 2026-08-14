# Maintaining this page

Everything here was learned by measuring, and most of it cost a round to find. Read the
part you are about to touch. `README.md` holds the plates and the verbatim rules; this
file holds the mechanics.

The whole site is one file, `index.html`, with no build step. GitHub Pages serves it on
push to `main`, which takes about a minute.

---

## 1. The photographs load in two stages

Nine of the ten plates are only ever seen as a **22% opacity wash through
`grayscale(1) contrast(.82)`**. They used to load the full colour photograph to render
that, which cost **1,439 KB across ten simultaneous requests and a 7.9 second load** on a
throttled phone.

`loading="lazy"` cannot help here and never could: every plate sits inside the viewport
from the first frame, so there is nothing for the browser to defer. It was behaving
correctly. **The weight has to come out of the files.**

So each cover also exists at `assets/covers/ghost/<name>.webp`, 420px wide and greyscale,
8 to 22 KB. Plates 1 to 9 load those and carry the real file in `data-full`. Plate 0 keeps
its full photograph, because it is the largest paint on the screen and the first to come
into register.

A plate swaps to its real photograph when it comes into register:

- `wantFull(i)` waits **260ms** before fetching, so a sweep does not drag the whole set
  down. Sweeping all ten takes 289ms; without the wait that fetched all ten photographs,
  about 1.1 MB, 45 seconds to settle on a slow connection. With it, a sweep costs one.
- `bringFull(i)` decodes the new file **off screen first** and only assigns `img.src` once
  it is ready, so the visible image never blanks. The plate transition is 820ms, which
  covers the swap.
- `fullDone[i]` stops a repeat. `onerror` clears it so a later visit can retry, and
  `data-full` is removed only after the assignment succeeds.
- If the network fails the ghost simply stays. A degraded picture, not a broken one.

**Regenerating the ghosts** (after replacing a cover):

```bash
python -c "
from PIL import Image
import os,glob
os.makedirs('assets/covers/ghost',exist_ok=True)
for p in glob.glob('assets/covers/*.webp'):
    Image.open(p).convert('L').resize((420,236),Image.LANCZOS).save(
        'assets/covers/ghost/'+os.path.basename(p),'WEBP',quality=52,method=6)
"
```

The tenth plate is the portrait at `assets/ali-al-mokdad.webp`; its ghost sits in the same
folder under that name. `ghost/overhead.webp` is unused, because plate 0 never needs one.

Measured effect, live, on 1.6 Mbps with 150ms RTT and 4x CPU throttling:
**1,540 KB → 339 KB, load event 7.9 s → 2.0 s.** Rendered both versions and diffed them:
mean difference 0.23 per channel out of 255, which is one tenth of one percent.

---

## 2. Two invariants that break silently

**The footer width.** The four control-strip patches overlap their borders by 1px and the
share bar is set to exactly their combined width, so:

```
share width = 4 × cell − 3
```

Both read from `--cell`, set on `.foot`. Never hand-write the two numbers separately; that
is how the alignment gets broken by an unrelated later edit. Phone tiers: 35px, then 32px
under 700px tall, 30px under 660px.

**The corner marks define one rectangle.** All four sit at 4px on phone and 8px on laptop.
This only holds because `.sheet` has `min-height:100svh` on phone. When the sheet was
content height, the bottom pair followed the content instead of the screen and sat 36px in
at 390x844 while the top pair sat 12px in.

---

## 3. The CSS cascade trap

There is a **deliberate final cascade at the end of the stylesheet** for phone bottom
spacing, share sizing and `--cell`. It is last on purpose.

This has been hit more than once: a broad `@media (max-width:700px)` block placed after a
height-gated tier silently overrides every tier, and an 812-tall screen inherits the value
meant for an 844 and scrolls by 19px.

**If you add a phone rule that competes with those, it goes after them, or it loses.**

---

## 4. Things that look wrong and are not

Do not "fix" these. Each was checked.

- **Ten hidden links in `nav.index`.** They are clipped to 1x1 but expand into a visible
  chip on `:focus-visible`. That is the correct focus-reveal pattern, not lost focus. The
  chip wraps and is anchored by its bottom so a two-line title grows upward rather than
  onto the scale; without the wrap, seven of ten ran off a 390px screen.
- **`.dial:focus{outline:none}`.** The indicator is a sibling, `.rulefocus`, revealed on
  `:focus-visible`. Measuring `outlineWidth` on the input reports nothing and means nothing.
- **Footer targets below 44px.** Deliberate, Ali's instruction. The smallest tier is 30px,
  above the 24px WCAG 2.2 AA floor. 44px is the AAA/Apple figure, not the AA one.
- **Consecutive identical anchors while tabbing.** Not a keyboard trap. The order cycles
  correctly through 19 focusables.

---

## 5. Behaviour that must stay true

- **The arrows must not lie.** `aria-disabled` is a statement, not a behaviour: the browser
  still fires the click. Both handlers return early when the button says it has nowhere to
  go. Before that guard, the greyed prev arrow still moved to plate 01 at the opening
  state, and the greyed next arrow still played a sound at the last plate.
- **`actx.resume()` returns a promise**, so a surrounding try/catch cannot see a rejection.
  Both call sites go through `resumeCtx()`, which swallows it. A browser that refuses to
  resume gets silence, not a console error.
- **Four synthesised sounds**, one family separated by octave, root G: link 3136Hz, next
  1568Hz, share 784Hz, read 196Hz. No audio file is ever fetched. Nodes must drain to zero
  after a burst; the iOS unlock buffer has to be disconnected or they do not.

---

## 6. The share card

`og:` and `twitter:` are both stated outright rather than relying on fallback, because X,
Slack, Signal, Telegram and Discord each resolve the fallback slightly differently.

**WhatsApp bakes the preview into the message at send time.** A message sent before a
change will show the old card forever and nothing on the server can reach it. To check a
change, type the link into a new message, or use the Facebook Sharing Debugger.

---

## 7. Verifying a change

The page must never scroll on a phone and must never overflow horizontally. Both are easy
to break and neither is visible without measuring.

Run a local server, then check, in this order:

1. **Look at the render.** Screenshot at 390 and 1440 and actually read them. Every single
   time this was applied it found something the numbers had passed.
2. **11 screen sizes** down to 360x640: no overflow, no scroll, share flush with the strip.
3. **axe-core** should report 0 violations and 37 passes.
4. **Console** clean at phone, laptop and reduced motion.
5. **The sounds**: four distinct voices, nodes drained to zero after a burst.
6. **The swap**: step through plates and confirm each becomes the sharp photograph without
   blanking, and that a fast sweep costs one image rather than ten.

Baseline to beat, measured live on throttled mobile: LCP under 600ms, CLS 0, total under
400 KB, load event around 2 seconds.
