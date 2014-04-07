---
layout: post
title: css-polyfills.js do more with CSS!
tags: css css-polyfills
---

I often hear "CSS is meant to style and HTML should describe content", but CSS alone is not enough to make textbooks.

Sometimes you need to:

1. style an element based on what's inside (notes containing a title)
2. change link text based on the target (`See Figure 2a` vs `See Table 4.3`)
3. move elements somewhere else in the book (answers to the back)

Well, now there is **[css-polyfills.js](/css-polyfills.js/)**, based on some great [CSS3](http://www.w3.org/TR/css3-content/) [specs](http://www.w3.org/TR/css3-gcpm/), [Sizzle](http://sizzlejs.com), and [selector-set](https://github.com/josh/selector-set).


## Simple Examples (from Bootstrap)

With **[css-polyfills.js](/css-polyfills.js/)** you can do things that are not possible using CSS in browsers. Bootstrap's [Dismissable Alerts](http://getbootstrap.com/components/#alerts-dismissable) require adding a special class on the alert if it contains a close button. This can be accomplished without adding the `alert-dismissable` class by using `:has`:

    .alert:has(> .close) { /* Styles for `.alert-dismissable` */ }

Bootstrap [Modals](http://getbootstrap.com/javascript/#modals) and [Input Groups](http://getbootstrap.com/components/#input-groups-buttons) require adding wrapper elements in order to style properly; with nested `::before` and `::outside` these are unnecessary.

You can construct the 2 additional elements (`.modal`, `.modal-dialog`) around `.modal-content` using the following CSS:

    .modal-content                  { ... }
    .modal-content::outside         { /* Styles for `.modal-dialog` */}
    .modal-content::outside::outside{ /* Styles for `.modal` */ }


## More Examples

You can even style links depending on the target:

    a[href^="#"] {
      // Use target-is as a guard for which link text to use
      content: target-is(attr(href), 'figure')   'See Figure';
      content: target-is(attr(href), 'table')    'See Table';
      content: target-is(attr(href), '.example') 'See Example';
    }

Or, you can move elements down the page (ie collate footnotes at the bottom of a wikipedia article):

    .footnote { move-to: footnotes-area; }
    #footnotes {
      content: pending(footnotes-area);
    }

You can also create the linkable footnotes on wikipedia by keeping the references near the content and use CSS to create links and move the references to the bottom of the page (See [Clickable Footnotes](/css-polyfills.js/#section-footnotes)).

Of course, there's much more that can be accomplished using [css-polyfills.js](/css-polyfills.js); check the [README.md](https://github.com/philschatz/css-polyfills.js) for more details.






