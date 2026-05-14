---
title: "Master Jekyll Feature Reference"
date: 2026-05-13
categories:
  - meta
tags:
  - jekyll
  - markdown
  - foam
  - formatting
  - reference
toc: true
toc_sticky: true
excerpt: "A master reference for which Jekyll/CommonMark/minimal-mistakes/Foam features render correctly in this garden's publishing pipeline. Validated empirically. Use this as a template for future formatting decisions."
---

This is a permanent reference for the garden's publishing capabilities. It exercises every formatting feature in one place. When in doubt about whether something will render correctly, check here.

The features are sorted by status:
1. Confirmed working (use freely)
2. Workarounds for broken features (use the workaround, not the broken syntax)
3. Things that don't work (avoid)

> **Note:** This file is for reference only. The content here is intentionally generic and abstract so search results pointing here don't pollute topic-specific queries.

---

## Working: Foam-style wikilinks

Foam in VSCode autogenerates standard markdown reference-link definitions at the bottom of each file inside HTML comment markers. The Jekyll pipeline processes them as normal reference links.

Inline wikilink: [[Foam]]

Multiple wikilinks in a sentence: This garden uses [[Foam]] for [[Digital-Garden]] management, with notes written by [[UltimApe]].

The aliased pipe syntax `[[Display text|target]]` does NOT work. Use bare `[[wikilink]]` only, or fall back to a regular markdown link if you need custom display text.

---

## Working: Smart quotes and dashes

The SMART option converts:
- Straight quotes to curly: "double quotes" and 'single quotes'
- Triple dots to ellipsis: ...
- Double hyphens to dashes: -- (becomes an en-dash)

Note that SMART converts em-dashes typed directly (—) into en-dashes (–) as well. If you need a true em-dash, use the HTML entity `&mdash;`.

---

## Working: Headings (H2 through H5)

### H3 looks like this

#### H4 looks like this

##### H5 looks like this

H1 is reserved for the post title. Start sections at H2.

---

## Working: Emphasis and inline formatting

Plain prose with **bold text** and *italic text* and ~~strikethrough~~ and `inline code`.

Inline code carries a background color and monospace font. Useful for technical references, identifiers, file paths, and short code snippets.

---

## Working: Lists

Unordered list with nesting:

- First item
- Second item with **bold inside**
- Third item with `inline code`
  - Nested item (use 2-space indent)
  - Another nested item
- Back to top level

Ordered list:

1. First
2. Second
3. Third

Nested ordered lists have indentation problems. Use 4-space indent if you must nest, or restructure with sub-headings.

---

## Working: Blockquotes

Single-line blockquote:

> The simplest form of a blockquote.

Multi-paragraph blockquote:

> First paragraph.
>
> Second paragraph in the same blockquote.

Blockquote as callout (the notice-box workaround):

> **Note:** Use bold-headed blockquotes instead of `.notice--warning` syntax. The notice classes fail because SMART converts `--` in attribute names to en-dashes.

> **Warning:** Same pattern works for warnings.

> **Important:** And for important callouts.

---

## Working: Code blocks

Inline code: `function_name()`, `variable`, `/path/to/file`.

Fenced code block:

```python
def example():
    return "fenced code with language hint"
```

Indented code block (4-space indent, no fence):

    plain code, no syntax highlighting

---

## Working: Links

Inline link: [example link](https://example.com).

Autolink (bare URL): https://example.com

Reference-style link: [example][ref1]

[ref1]: https://example.com "Optional title"

---

## Working: Footnotes

Sentence with a footnote.[^one]

Multiple footnotes back to back.[^two][^three]

Footnote with formatting and a link inside.[^complex]

[^one]: This is a simple footnote.

[^two]: Footnote with **bold** and *italic*.

[^three]: Third footnote.

[^complex]: Footnote containing a [link](https://example.com) inside it.

Footnotes render as a numbered list at the bottom of the post with back-link arrows.

---

## Working: Tables

Basic GFM table:

| Column A | Column B | Column C |
|---|---|---|
| row 1a | row 1b | row 1c |
| row 2a | row 2b | row 2c |
| row 3a | row 3b | row 3c |

Note: column alignment syntax (`:---:`) is ignored. Tables render with default alignment regardless.

---

## Working: Images with captions

Plain image syntax with italic caption on the next line. This replaces the broken `{% include figure %}` include.

![Alt text describing the image](/assets/images/example.png)
*Caption text in italics, on the line below the image.*

The minimal-mistakes figure include leaks `</figcaption></figure>` HTML into the output and should not be used.

---

## Working: Unicode emoji

Use literal unicode characters instead of `:shortcode:` syntax:

Common test emoji: 🌱 🦋 📚 🔬 🧪

Inline use: This is a test 🌱 of inline emoji rendering.

Shortcode syntax (`:bear:`) renders as broken inline images. Always use unicode directly.

---

## Working: Horizontal rules

Three or more hyphens on a blank line:

---

That renders as a horizontal line.

---

## Working: Hard line breaks

Two trailing spaces force a hard line break.

Line one  
Line two (preceded by two trailing spaces on line one)

Without trailing spaces, lines flow into the same paragraph:
Line three
Line four (joins line three in rendered output)

---

## Working: Hashtags as plain text

Inline hashtag: `#example` does NOT trigger heading processing because it's inside inline code.

In prose: a hashtag like #example written normally also does not trigger headings, since CommonMark requires the `#` to be at the start of a line.

---

## Broken: Notice boxes

The `.notice--variant` syntax does NOT work because SMART converts `--` in the class attribute to an en-dash, breaking the CSS class match.

Do not use:
- `{: .notice}`
- `{: .notice--primary}`
- `{: .notice--info}`
- `{: .notice--warning}`
- `{: .notice--success}`
- `{: .notice--danger}`

Use bold-headed blockquotes instead. See the working example above.

---

## Broken: Emoji shortcodes

Do not use `:emoji_name:` syntax. It renders as a tiny broken-looking inline image from GitHub's emoji CDN with the shortcode text visible alongside.

Use literal unicode characters directly.

---

## Broken: Collapsible details

The HTML `<details>` and `<summary>` tags pass through (UNSAFE is enabled), but the disclosure widget doesn't get any interactive styling. The summary text renders as bold inline text, and the content shows expanded by default.

Workaround: use a horizontal rule and a "section break" heading to separate optional content. Readers can skip it visually.

---

## Broken: Keyboard key styling

The `<kbd>` HTML tag renders as plain inline code without any keyboard-key styling.

Use backtick inline code instead: press `Ctrl+C` to copy.

---

## Broken: Inline HTML style attributes

Inline `<span style="...">` styling is stripped by tagfilter. Text appears, but the style attribute is removed, so the styling has no effect.

Use bold or italic for emphasis. There is no inline color support.

---

## Broken: Aliased wikilinks

The pipe syntax `[[Display Text|target-page]]` does NOT work. Renders as literal text with brackets.

Use bare wikilinks `[[target-page]]` (the title from the autogenerated reference table becomes the link text) or fall back to a standard markdown link `[Display Text](url)` when custom display text is needed.

---

## Broken: Figure includes with captions

The `{% include figure %}` Liquid include partially renders but leaks `</figcaption></figure>` HTML into the visible page output.

Use plain image syntax with italic caption on the next line instead.

---

## Special: Edge cases

Real em-dash typed directly: this — and this — both get converted to en-dashes by SMART.

Triple backticks inside text: `` `inline code containing a backtick` `` works fine with the double-backtick escape.

Special characters: `&`, `<`, `>`, `"`, `'` all render as themselves.

URL with fragment: https://example.com/page#section renders as a proper autolink with the fragment preserved.

---

## Front matter reference

```yaml
---
title: "Post title in quotes"
date: 2026-05-13
categories:
  - category-name
tags:
  - tag-one
  - tag-two
  - tag-three
toc: true
toc_sticky: true
excerpt: "Short description for the post preview on the index page."
---
```

Fields that work:
- `title` — page title
- `date` — publication date (ISO format)
- `categories` — list of categories, becomes part of the URL
- `tags` — list of tags
- `toc: true` — auto-generates table of contents from headings
- `toc_sticky: true` — TOC follows scroll position on the right side
- `excerpt` — preview text on index pages

---

## Style cheatsheet

| Need | Use |
|---|---|
| Internal link to another garden post | `[[target]]` plus reference table entry |
| External link | `[text](url)` |
| Quote from a tweet or post | Blockquote with em-dash attribution |
| Safety / warning callout | Blockquote with `**Bold header:**` |
| Image | `![alt](path)` then `*caption*` on next line |
| Emoji | Literal unicode character |
| Footnote | `[^id]` inline, `[^id]: definition` at bottom |
| Code reference inline | Backticks |
| Multi-line code | Fenced block with language hint |
| Table | GFM pipe syntax, no alignment |
| Emphasis | Bold or italic only |
| Section break | Horizontal rule on its own line |

---

## Foam reference table convention

Append the autogenerated reference table at the bottom inside HTML comment markers. Foam regenerates this on save in VSCode. When writing outside Foam, include reasonable starter paths so the post is functional immediately.

```markdown
[//begin]: # "Autogenerated link references for markdown compatibility"
[Foam]: weeds/software/tools/foam.md "Foam"
[Digital-Garden]: weeds/meta/digital-garden.md "Digital Garden"
[UltimApe]: weeds/people/person/ultimape.md "About: UltimApe"
[//end]: # "Autogenerated link references"
```

The third field in each reference (the quoted title) becomes the visible link text on the rendered page. The brackets are preserved as part of the wikilink styling.

---

[//begin]: # "Autogenerated link references for markdown compatibility"
[Foam]: ../software/tools/foam.md "Foam"
[Digital-Garden]: ../meta/digital-garden.md "Digital Garden"
[UltimApe]: ../people/person/ultimape.md "About: UltimApe"
[//end]: # "Autogenerated link references"
