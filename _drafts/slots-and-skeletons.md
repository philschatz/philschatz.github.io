---
layout: post
title: Pattern matching with LESS (CSS)
---

# Background

Pattern-matching mixins is **the** distinguishing characteristic that separates Less from other CSS preprocessors like Stylus and Sass.

> Let the compiler do what it's good at: match templates and error early (when one is missing).

LESS is a beautiful language (like CSS) _precisely_ because it is **not** Turing-complete.
Instead, it explores just how far you can take a language with those restrictions.

For example, you can provide conditionals using the pattern-matching `when` keyword instead of relying explicit control flow like `if then else`. You can match arguments to mixins either by value or with `...` (like Ruby, CoffeeScript, and recent versions of Java).

This frees the author up to put conditional styles elsewhere in a file (much like XSLT templates). Similarly, mixins whose arguments match are always applied. This can initially be an annoying feature but for styling textbooks this is central in our design.

# Case Study: Making Textbooks

In textbooks we need to style elements differently depending on where they are (context is crucial).

A subfigure (`figure > figure`) should render differently than a `figure`.

A table should be numbered differently when it is in a chapter (ie `Table 4.3`) than when it is in an appendix (`Table A3`).

Or take problems and solutions: a problem and solution at the end of a chapter should be "treated" differently than an example with a worked out problem and solution. The former should be numbered (and solution should be moved to the back of a textbook) while the latter should be included in the example (since it is a worked-out example).

Or take a definition of a term. If an equation is used then it should not be numbered; referring to an equation inside a definition does not make a whole lot of sense.

In each of these cases it is the **context** that is important when deciding how to style an element.

Additionally, the HTML elements differ for different outputs; online the CSS selectors are different than for a single-page book or a multi-page EUPB.

# Solution: Slots and Skeletons

To handle multiple HTML formats (EPUB, online, PDF) we came up with the idea of separating the CSS selectors (skeleton) from the rules (slots).

Each book contains a very similar HTML structure (depending on the output format; PDF, EPUB, online) but very different styling (fonts, colors, numbering schemes) so we split the CSS into 2 types of files: **slots** for the styling and **skeletons** for the HTML structure and linked them together using a namespaced tree of mixins that represent the _logical_ structure of a book.

In this way a page header would be defined as (the _slot_):

    #page>#top>.outside() { content: 'Chapter 1'; }
and used as (the _skeleton_):

    @page:left { @top-left: #page>#top>.outside(); }

This allows us to separate the styling (slots) from the content (skeleton).

## Context

Getting back to pattern matching; the context of an element (ie "Exercise") is important for styling that element.
To accomplish this, we created rules for defining mixins and special "generators" for all the possible permutations of contexts.

In our system a mixin has roughly the following signature: `#logical-type>.mixin(@kind; @part; @contexts...) { }`.

Let's go through the various parts:

- `#logical-type` roughly corresponds to the structural element (and `data-type` attribute). Some examples are `exercise`, `note`, `term`, `example`
- `.mixin(...)` is the kind of slot (`.style(...)` for the whole element, `.title(...)` for the title, `.numbering(...)` for how to number the element, etc)
- `@kind` corresponds to a specific class for a particular book (a science book may have `experiment` while a history book may have `did-you-know`)
- `@part` represents which part of a book the element occurs in (`preface`, `chapter`, `appendix`, `any`) and is largely used for numbering an element
- `@contexts...` represents what this `logical-type` occurs inside (and where all the fun happens). Some examples would be `#figure>.style(any; any; figure) { }` (a subfigure), or `#table>.style(any; any; glossary)` (a table inside a glossary)


This gives us the flexibility to style the content of a book based on the logical parts (using namespaces) and the context it occurs in (using `@contexts...`) and have it work for different output formats (by having different skeleton files).

To accomplish the `@contexts...` bit we created some mixins that generate all permutations of different contexts. This is accomplished by adding list-expansion to LESS which allows us to expand a list when calling a mixin. For example:

    .context-expander(@head; @tail...) {
      .mixin-call(@tail...);        // <-- The new piece added to LESS
    }

**Note:** For most of these permutations, no mixins will match so they will be omitted from the generated CSS.

The result is a single skeleton file for each output format (EPUB, PDF, online) and a slots file (~100 lines) for each book.

