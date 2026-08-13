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
| 08 | a collapsed bridge sinking into a calm green river | risk |
| 09 | a wall papered with hand drawn Like and dislike buttons | digital |
| 10 | Ali Al Mokdad against a yellow perforated wall | the person who noticed |

So the photographs are the argument, and the site is built to serve them rather than to
decorate around them. Misregistration is a real printing defect and these are real press
photographs, which is why the sheet, the trim marks, the plate slugs and the millimetre
scale are the visual language: they are what you actually use to check register.

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
- The ten quoted lines are exact sentences located in his drafts.

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

- The interface is a native `<input type="range">`. Arrow keys, Home, End, number keys
  1 to 9 and 0, touch drag and screen readers all work without anything being
  reimplemented. There is no custom knob widget.
- All ten pieces exist as real links in the document at all times, so every one is
  reachable without a menu existing.
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
