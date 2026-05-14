---
title: "Jekyll Feature Test — The Fast Publishing Format"
date: 2026-05-13
categories:
  - test
tags:
  - test
  - jekyll
  - formatting
toc: true
toc_sticky: true
excerpt: "A test post to verify which Jekyll/CommonMark/minimal-mistakes features render correctly in the garden's publishing pipeline."
---

This file tests every formatting feature we plan to use in the main post. Drop it into the garden's `_weeds/` directory, push, and review what renders. Anything broken gets removed from the main post's format.

## 1. Basic prose and emphasis

Plain prose paragraph with **bold text** and *italic text* and ~~strikethrough~~ and `inline code`.

This paragraph tests dialogue voice and apostrophes. I'm checking that smart quotes work. "Quoted speech with smart quotes." 'Single quotes too.' Auto-converted from straight quotes by SMART.

Em-dashes from double hyphen: this -- right here -- should become an em-dash via SMART. Even though we're stylistically avoiding them, the engine handles them.

Triple-dot ellipsis from `...` should become a real ellipsis... like that.

## 2. Headings

### H3 looks like this

#### H4 looks like this

##### H5 looks like this

## 3. Lists

Unordered list:

- First item
- Second item with **bold**
- Third item with `code`
  - Nested item
  - Another nested item
- Back to top level

Ordered list:

1. First
2. Second
3. Third
   1. Nested ordered
   2. Another nested

## 4. Blockquotes

> This is a single-line blockquote.

> This is a multi-paragraph blockquote.
>
> Second paragraph in the same blockquote. Useful for direct tweet quotes from source files.

> **Bolded quote attribution:** "I cured my treatment resistant major depression by flushing out my gut and then fasting, back in 2018."
> — @ultimape, Dec 9 2022

Nested blockquote:

> Outer quote.
>
>> Inner nested quote, in case we ever need to quote someone quoting someone.

## 5. Code

Inline code: `rs7997012` and `L. reuteri` and `pip install pandas`.

Fenced code block with language:

```python
def microbiome_diversity(samples):
    return shannon_index(samples)
```

Indented code block:

    Plain code block, no syntax highlighting.

## 6. Links

Inline link: [garden FMT post](https://garden.wovensoup.com/weeds/write-drafts/longform/dog-shit-n-of-1/that-dogshit-in-me).

Autolink (bare URL): https://pmc.ncbi.nlm.nih.gov/articles/PMC10933111/

Reference-style link: [Hu et al. 2024][hu2024]

[hu2024]: https://translational-medicine.biomedcentral.com/articles/10.1186/s12967-024-05816-1 "From inflammation to depression"

Wikilink internal: [[that-dogshit-in-me]] — tests whether wikilinks resolve to other garden posts.

Wikilink with display text: [[FMT story|that-dogshit-in-me]] — tests aliased wikilink.

## 7. Footnotes

Sentence with a footnote.[^one]

Another sentence with a different footnote.[^paper]

A sentence with two footnotes back to back.[^two][^three]

[^one]: This is the first footnote definition.

[^two]: Second footnote with **formatting** inside.

[^three]: Third footnote with a [link inside](https://example.com).

[^paper]: Hu et al., "From inflammation to depression: key biomarkers for IBD-related major depressive disorder," *Journal of Translational Medicine* 22, 2024. https://translational-medicine.biomedcentral.com/articles/10.1186/s12967-024-05816-1

## 8. Tables

Basic GFM table:

| Outcome | Pre-fast | Post-fast | Change |
|---|---|---|---|
| Grip strength | 40 lbs | 60 lbs | +50% |
| Weight | 264.7 lbs | 251.2 lbs | −13.5 lbs |
| Body fat | 33.9% | 31.6% | −2.3% |
| Depression episodes (30 days) | daily | 0 | n/a |

Table with alignment:

| Left | Center | Right |
|:---|:---:|---:|
| left-aligned | center | right |
| more text | here | also right |

## 9. Notice boxes (minimal-mistakes)

This paragraph tests the default notice box.
{: .notice}

Primary notice (blue).
{: .notice--primary}

Info notice (light blue).
{: .notice--info}

Warning notice (yellow). Good for safety callouts.
{: .notice--warning}

Success notice (green).
{: .notice--success}

Danger notice (red). Good for "do not try this at home."
{: .notice--danger}

Multi-line notice test:

**Safety note:** This was not improvised. The protocol was designed over a year, with $200 of monitoring equipment, four designated safety contacts, and a graded escalation ladder. Someone who tries to wing this without the preparation is not doing the same experiment.
{: .notice--warning}

## 10. Images

Basic image (file would need to be uploaded to `/assets/images/`):

![Test alt text](/assets/images/test-image.png)

Minimal-mistakes figure with caption:

{% include figure image_path="/assets/images/test-image.png" alt="Test figure" caption="A test caption beneath the image." %}

## 11. Emoji shortcodes

Bear: :bear:

Brain: :brain:

Poop: :poop:

Mushroom: :mushroom:

Microbe: :microbe:

Test of inline use: I just want to hibernate :bear: lol.

## 12. Horizontal rules

Standard rule:

---

That should render as a horizontal line.

## 13. Raw HTML (UNSAFE enabled)

<details>
<summary>Click to expand a collapsible section</summary>

This content is inside a collapsed `<details>` block. Useful for the appendices or for "show your work" sections that some readers want and others want to skip.

Includes **markdown formatting** inside.

- List item one
- List item two

</details>

Inline HTML for tooltips or styling:

This is <span style="color: red;">red text</span> via inline style.

<kbd>Ctrl</kbd> + <kbd>C</kbd> renders as keyboard keys.

## 14. Hard line breaks

Line one  
Line two (preceded by two spaces — should be a hard break)

Line three (no trailing spaces, should not be a hard break)
Line four (continuation of same line in rendered output)

## 15. Edge cases

Hashtag in plain text: `#DeleteTwitter` — verify it doesn't get processed as a heading.

Em-dash already typed directly: This — right here — uses real em-dashes typed in (not double-hyphen converted by SMART).

Triple backticks inside text: like `` `inline code with backtick` `` works.

Special chars: & < > " ' — should render as &, <, >, ", '.

URL with fragment: https://pmc.ncbi.nlm.nih.gov/articles/PMC6007539/#s6title

## 16. Long-form prose paragraph test

The Citalopram withdrawal was the discovery engine. Trying to understand what was happening to me, akathisia, brain zaps, Lhermitte's sign, sent me into the genetics of drug metabolism, serotonin pathways, GABA, BDNF, NMDA. From there to fasting. The fasting bit specifically was inspired by emerging MS clinical trial work in 2017 and 2018. I'd been looking at my grandmother's MS and wondering if it was related to my own neurological symptoms. Lhermitte's sign shows up in both SSRI withdrawal and MS, and that overlap kept jumping out at me as a diagnostic clue. If the same symptom appears in both, they share an underlying mechanism. That pointed at the gut.

This tests how a 360-words-per-minute reader handles a real paragraph from the planned post. If this renders cleanly with proper spacing, kerning, and read-time calculation, the format works.

---

## Render checklist

After publishing this test file, verify:

- [ ] TOC generated automatically from headings
- [ ] TOC sticks to side while scrolling
- [ ] Smart quotes converted from straight quotes
- [ ] Em-dashes from `--` converted
- [ ] Ellipses from `...` converted
- [ ] All heading levels styled correctly
- [ ] Nested lists render with indentation
- [ ] Blockquotes have a left border
- [ ] Nested blockquotes work
- [ ] Code blocks have syntax highlighting (Python)
- [ ] Inline code has a background color
- [ ] Wikilinks resolve correctly to other garden posts
- [ ] Footnotes render at the bottom with back-links
- [ ] Tables render with borders and alignment
- [ ] All five notice box variants render with their colors
- [ ] Images render with captions (if uploaded)
- [ ] Emoji shortcodes convert to images
- [ ] Horizontal rules show
- [ ] `<details>` collapsible section works
- [ ] Inline HTML styling works
- [ ] `<kbd>` keys render with styling
- [ ] Hard line breaks (two trailing spaces) work
- [ ] Hashtags don't get treated as headings
- [ ] Read time is calculated and shown
- [ ] Excerpt appears on the index page
- [ ] Tags and categories are clickable
