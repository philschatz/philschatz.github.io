---
layout: post
title: css-polyfills for Books
tags: css-polyfills
---


## "Why CSS instead of Javascript?""

Wouldn't it be great if authors could customize their books without having them write (or run) arbitrary JavaScript? This post shows a way to do it.

## Our Problem

Our books end up being published in various formats with various support for CSS.

We use Docbook for PDFs partly because we need to move content around (ie collating exercises at the end of a chapter, making an index) and XSLT provides a way to move XML around.

Unfortunately, this means 4 things:

- developers need to learn XSLT
- we must regression-test all of our books whenever we fix a bug or add a feature
- CSS for the PDF is different for ePUB and online
- numbering things like exercises is different in a PDF than online

Fortunately, there are a few W3C Drafts that help fill in some of the gaps: [Generated Content for Paged Media](http://www.w3.org/TR/css3-gcpm/) and [CSS3 Generated and Replaced Content Module](http://www.w3.org/TR/css3-content/).


Intersection of Some CSS Features:

| Feature              | EPUB2   | Browsers  | PrinceXML (PDF) |
| :------------------- | :-----: | :-------: | :-------------: |
| `::before`           | **no**  | yes       | yes             |
| `counter-increment:` | **no**  | yes       | yes             |
| `content:`           | **no**  | _partial_ | yes             |
| `target-text()`      | **no**  | **no**    | yes             |
| `page-break-*:`      | **no**  | **no**    | yes             |
| `move-to:`           | **no**  | **no**    | **no**          |
| `::outside::before`  | **no**  | **no**    | **no**          |
| `:has()`             | **no**  | **no**    | **no**          |

To replace Docbook and have one CSS file to style the various formats we need to support all of these features **and** note a few differences:

- PDF is generated using a single large HTML file (CSS needs to operate on all chapters)
- ePUB needs to be chunked into multiple HTML files (ideally using CSS `page-break-*`)
- Online, a single HTML file can be viewed outside the context of a book


### Browsers

Ideally, we would be able to get access to all of these unsupported selectors and rules using the [CSS Document Object Model](http://www.w3.org/TR/DOM-Level-2-Style/css.html) but browsers only expose the selectors and rules they understand.

## The Solution

Enter [CSS-Polyfills](/css-polyfills.js/).

The project uses [LessCSS](http://lesscss.org) and [jQuery](http://jquery.org) to parse the CSS file and "bake" the changes into the HTML.

With it you can do things that are not possible using CSS supported by browsers. For example, you can style an element based on children inside:

    .note:has(> .title) { /* Give it a fancier border */ }

Or, you can automatically generate a glossary at the and of a chapter based on terms in the chapter:

    .term > .definition { move-to: glossary-area; }
    .chapter:after {
      content: pending(glossary-area);
    }

You can even style links depending on the target:

    a[href] {
      // Use x-target-is as a switch for which link text to use
      content: x-target-is(attr(href), 'figure')   'See Figure';
      content: x-target-is(attr(href), 'table')    'See Table';
      content: x-target-is(attr(href), '.example') 'See Example';
    }

In another post, I'll go over some of the "Freebies" that come out of this project like CSS Coverage and CSS+HTML Diffs for regression testing.


## Bonuses

By parsing the CSS file and "baking" the styles into the HTML there are a few "freebies" that come out.

### Easy CSS Coverage

As a free perk, you can easily generate Coverage data for your CSS files by transforming a HTML and CSS file from the commandline and filtering stderr.

### HTML+CSS Diffs

To do regression tests on books we merely need to generate the "baked" HTML file twice, once with the old CSS and once with the new CSS (all the styles are "baked" into `style="..."` attributes).
Then, a quick XSLT file can compare the two and generate a version of the page with `<span>` tags marking the places where styling changed.

See <https://github.com/philschatz/css-diff.js> for a package that does this.
