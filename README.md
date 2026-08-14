# smallccut.org

One page. Ten plates on a press sheet, nine of them out of register. Tuning the millimetre
scale pulls exactly one into true. You cannot correct them all, which is the argument.

Static HTML, one file, no build step. Two self hosted variable fonts. No third party
request of any kind, no analytics, no cookies, no tracker.

---

## Why it looks like this

The nine cover images are not stock photography. They are Ali Al Mokdad's own field
photographs, and each one is a dry observed joke about the exact problem its piece attacks:

| plate | photograph | piece |
|---|---|---|
| 01 | a lorry whose tarpaulin load towers far above the cab | overhead |
| 02 | one man hauling an overloaded cart of scrap through mud by hand | headquarters efficiency |
| 03 | a taxi driving with a full air conditioning duct unit strapped to its roof | strategy |
| 04 | a queue of identical white four wheel drives on a dirt road | transformation |
| 05 | a row of near identical tents in a dry field | regional offices |
| 06 | a market stall crammed floor to ceiling with stacked notebooks | fundraising |
| 07 | a decommissioned rocket shell reused as a bench | advocacy |
| 08 | a collapsed bridge sinking into a calm green river | risk management |
| 09 | a wall papered with hand drawn Like and dislike buttons | digitalization |
| 10 | Ali Al Mokdad against a yellow perforated wall | the person who noticed |

So the photographs are the argument, and the site is built to serve them rather than to
decorate around them. Misregistration is a real printing defect and these are real press
photographs, which is why the sheet, the trim marks, the plate slugs and the millimetre
scale are the visual language: they are what you actually use to check register.

Each plate carries ONE line of his prose, not two. It used to show the article opening
AND a line quoted from inside the piece, which said the same thing twice: across the ten
plates the openings run 236 words against the quotes 112, and several openings only make
sense mid article. The quote stays, the opening comes off the panel. Nothing is lost,
because the openings remain in the no JS block, which is the crawlable copy, and in
llms.txt and llms-full.txt.

The interface verb is correction, not addition. Everything unfixed sits there silently
wrong. It does not vibrate and it does not pulse. Motion appears only while a plate is
being pulled into true, and arrival is not celebrated: the corrected plate simply stops
being wrong.

## The palette is measured, not chosen

62,000 pixels were sampled across the nine photographs. Every token is a statistic of
Ali's own work:

| token | value | derivation | contrast on paper |
|---|---|---|---|
| paper | `#DFE0DF` | mean of the lightest 15% | |
| dust | `#82827D` | arithmetic mean of all sampled pixels | 2.92:1 |
| shade | `#3B3B36` | mean of the darkest 25% | 8.51:1 |
| ink | `#0F1010` | mean of the darkest 2% | 14.40:1 |
| signal | `#EEC51C` | mean of the most saturated 0.5% | 11.47:1 against ink |

`dust` measures 2.92:1, which fails WCAG AA, so it carries hairlines and scale ticks only
and never text. Text is `ink` or `shade`.

The signal yellow is the most saturated colour anywhere in his nine photographs, which is
the taxi in plate 03. The yellow wall behind his head in plate 10 measures `#F9D511`, a
distance of 44 out of a possible 765 in summed RGB channels, under 6%. The colour that
means "in register" across the whole page turns out to be the colour behind his head, and
that relationship was measured rather than asserted.

## Every word on the page is his

- The epigraph is his definition, verbatim: *"Small c" is the term I use for quiet,
  strategic reductions that unlock real change over time.*
- The ten titles are exactly as published.
- The ten opening lines are exact complete sentences from the published pieces, or from
  his own drafts for plate 01 and plate 10.
- The quoted lines are exact sentences located in his drafts, with one stated exception.
  Plate 10 reads “Starting from first principles. Optimization in small steps is what frees
  an organization to take the big ones.” The second sentence is verbatim from his drafts,
  where it follows “My take remains simple.” The opening four words are NOT from the drafts:
  Ali wrote them for this plate on 14 August 2026, so they are his, just newly written rather
  than quoted. Only the capital S was added, to match the sentence case every other quote uses.
- One deliberate exception to keep in mind before “fixing” it. Plate 09 quotes
  “Technology isn’t the problem. Transformation is.”, which is verbatim his but comes
  from the STRATEGY draft, where it is a heading in capitals, not from the digital piece
  the plate links to. Ali chose that line for this plate. The words are unchanged and only
  the case differs. It is therefore not findable in the linked article, which is the known
  and accepted cost. His digital piece has its own line for the same argument, “The
  technology was never the point.”, if it is ever swapped back.

Nothing is paraphrased into his voice and nothing was written to sound like him. Two
opening lines in an early draft of this page were mine and were replaced with verbatim
ones during review. Where an em dash appears it is inside quoted source text, in his
published title for plate 08 and his published opening for plate 03, and is left exactly
as he published it.

## Reproducing the checks

Everything asserted above is reproducible. The working scripts live in the session
scratchpad, not in this repository, but the checks are simple to restate:

1. **Palette.** Sample the nine JPEGs in `assets/covers/`, take the mean of all pixels,
   of the lightest 15%, of the darkest 25% and 2%, and of the most saturated 0.5% by
   `saturation * (1 - |2L - 1|)`. Compute WCAG contrast for each against paper.
2. **Verbatim strings.** For each of the 20 titles and opening lines, assert the string is
   a literal substring of either the article's `og:title` / `og:description` or of the
   text extracted from his drafts. The build refuses to write if any one of them fails.
3. **No drift.** Assert every title and every quoted line appears in both the interactive
   layer and the static fallback.
4. **Accessibility.** axe-core, WCAG 2.0 and 2.1 A and AA plus best practice: zero
   violations, 32 passes. The remaining "incomplete" items are contrast probes that axe
   cannot compute under the `::after` register grid, plus two glyph only `kbd` elements.

## Accessibility and robustness

- The interface is a native `<input type="range">` with a step button either side. Arrow
  keys, Home, End, number keys 1 to 9 and 0, touch drag, clicking a plate and clicking a
  step all work, and none of it is a reimplemented widget. The steps carry `data-end` and
  `aria-disabled` at the extremes rather than the `disabled` attribute, so they keep their
  place in the tab order and are still announced.
- All ten pieces exist as real links in the document at all times, so every one is
  reachable without a menu existing. They sit after the two steps in the document, so a
  keyboard journey runs scale, steps, then the ten titles; a focused one surfaces as an ink
  slug above the instrument and does not reflow the scale.
- The four links out (his site, LinkedIn, X, YouTube) are a control strip in the footer:
  contiguous ink cells with the mark knocked out in paper, inverting on hover and focus.
  34px cells, 44px under a coarse pointer.
- On phones the plate bleeds off both sheet edges, and two short-screen blocks tighten the
  vertical rhythm so the whole page lands in one view down to 360x740. At 375x667 it is
  8px over with nothing important below the fold.
- The wheel is deliberately left alone. Nothing is scroll jacked.
- `prefers-reduced-motion` reduces every transition to 1ms and skips the opening
  demonstration, so a reduced motion visitor is never left looking at an unexplained
  out of register sheet.
- `prefers-contrast: more` lifts paper to white and darkens the secondary text.
- With JavaScript unavailable the ten pieces are served as plain content with their
  photographs, titles, quoted lines, openings and links. That block is also the crawlable
  copy of the page, so the site says what it is in the HTML itself.

## Deployment

GitHub Pages, custom domain via `CNAME`. `smallccut.org` is registered to Ali Al Mokdad at
Namecheap. DNS is Namecheap BasicDNS with four A records to GitHub Pages and a CNAME on
`www`.

## Credits

Photographs and all text: Ali Al Mokdad. alialmokdad.com

Fonts: Archivo and JetBrains Mono, both SIL Open Font License, subset to latin and self
hosted in `assets/fonts/`.
