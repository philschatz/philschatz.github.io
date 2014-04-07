---
layout: post
title: Building CSS skeletons and slots
tags: css
---

## Simple Slots and Skeletons

We are transitioning from 1 output format for CSS (Docbook HTML) to multiple HTML formats (PDF, EPUB, web).

To reuse the same CSS we split the selectors and rules into mixins that are namespaced based on their logical part in the book. Instead of a `.note { color: blue; }` we split it into 2 parts:

    // The skeleton:
    .note { #content>#note>.style(); }

    // and the slot:
    #content {
      #note {
        .style() { color: blue; }
      }
    }

This allows us to reuse the logical styling when the HTML format changes.

Now, styling a book merely involves _filling in_ the right slots.

## Slot parameters

### Custom classes

Often we have custom "features" in a book. These are often written and as notes or sections with a custom class name.

To style these, every slot has a `@kind` parameter as the 1st argument to the mixin.
Here is an example of a "How To" feature:

    #content {
      #note {
        .style('how-to') { color: orange; }
      }
    }

### Parts of a book

But sometimes it is necessary to style the feature differently based on which part of the book it is in. Most commonly this is used for numbering. For example, a Figure in a chapter would be labeled `Figure 4.3` but in an appendix it would be labeled `Figure A3`.

To support this, the 2nd argument is the `@part`. For _most_ slot definitions this will just be `any`.

## Contexts

When styling a book, it is often important to know what an element is in. For example, a worked-out example is actually an `exercise` inside an `example`. It should not be numbered (since it is not a homework problem) and might be rendered with text other than `Problem` and `Solution`.

To handle this case, the final arguments to a mixin are `@contexts...`.

Here is how to style an exercise inside an example:

    #content {
      #exercise {
        .style(any; @part; example) { color: green; }
      }
    }

This makes `any` exercise (regardless of class) in any `@part` that occurs inside an `example` green.

If you wanted to make any exercise inside a `how-to` example yellow you would write:

    #content {
      #exercise {
        .style(any; @part; example; 'how-to') { color: yellow; }
      }
    }

Similarly, any exercise in an example in the `review-questions` section at the end of a chapter would be something like:

    #content {
      #exercise {
        .style(any; chapter; end-of; section; 'review-questions'; example) { color: aqua; }
      }
    }


That's about it for the intro to slots and skeletons. I should have another post on the namespace organization of a book and a post on how the machinery needed to get the `@contexts...` parameter to work.
