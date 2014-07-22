---
layout: post
title: Skeleton Auto-generation
tags: css
---

In [Building CSS skeletons and slots](/2014/03/16/slots-and-skeletons-intro/) I showed how to style a piece of content based on where it occurs (the `@contexts...` parameters).

The code can be found at **[philschatz/skeleton-generator.css](https://github.com/philschatz/skeleton-generator.css)**.

Here's a discussion on the machinery needed to realize that.

# Types and Templates

First, we need a way to describe the selector for a particular type. Let's use a subfigure (figure inside a figure) for this example.

A typical figure would have styling like:

    figure {
      border: 1px solid blue;
      figcaption {
        color: green;
      }
    }

When split into slots and skeletons (let's ignore `@kind` and `@part` for now) it would look like:

    // The "skeleton":
    figure {
      #content>#figure>.style();
      figcaption {
        #content>#figure>.caption();
      }
    }

    // The "slots":
    #content {
      #figure {
        .style()    { border: 1px solid blue; }
        .caption()  { color: green; }
      }
    }

Now, we have something to work with.
To make a _subfigure_ have a `yellow` border, the CSS would look something like:

    figure {
      figure {                // Note: it's inside another figure
        border: 1px solid yellow;
      }
    }

and the skeleton and slots would look like:

    // The "skeleton" for a subfigure:
    figure {
      figure {
        #content>#figure>.style(figure); // the arg denotes that this is
                                         // _inside_ another `figure`
        figcaption {
          #content>#figure>.caption(figure);
        }
      }
    }
    // The "slots" for a subfigure:
    #content {
      #figure {
        .style(figure) { border: 1px solid yellow; }
      }
    }

Now, we could write all possible permutations of figures, notes, tables, examples, etc in the skeleton file but that would be _tedious_. Instead, we can autogenerate them using the `...` list expander in LessCSS.

First, we need to break up the figure into a couple pieces:

1. the selector that distinguishes a `figure` from a `.note` or a `.example`
2. the template of the "structure" inside a figure (it has a `.style()` and `.caption()`)

For each `@type` (figure, note, table) the selector can be defined as:

    #skeleton {
      .selector(figure) {
        figure { .x-template-helper(figure); }
      }
    }

Assuming `.x-template-helper` does something (for now), this would allow us to create:

    figure {
      figure {
        ...         // figures all the way down!
      }
    }

... since the only thing this mixin defines is the selector `figure { ... }` (so it can be nested).

Next, we need to define what can be inside a figure (or subfigure). This is defined by the `#skeleton>.template(figure)` mixin. We can define it as:

    #skeleton {
      .template(figure) {
        // All figures have a style and may have a caption.
        #content>#figure>.style();

        figcaption { #content>#figure>.caption(); }
      }
    }

This says a figure has a `.style()` and a style on the caption.

So far we have defined both what a figure is (the `.selector(@type)`) and what it can contain (the `.template(@type)`).

Now we need to combine these to generate the expected CSS:

    figure {
      figure {
        border: 1px solid yellow;
      }
    }

To do it, we need to build a bit more machinery first!

# List evaluation in LessCSS

There are several `@types` we need to permute. In this example there are only figures, but for a book there will be several types (ie `figure`, `note`, `table`, `example`).

The generator needs to iterate over all these types so for now let's put them in a variable.

    @all-types: figure, note, table, example;

LessCSS has a way way of going through and recursively evaluating a list:

    .recursive() { } // base case
    .recursive(@head; @tail...) {
      content: @head;
      .recursive(@tail...); // The `@tail` list is expanded to
                            // multiple arguments
    }

Using this structure and the `.selector(@type)` and `.template(@type)` mixins we can create the following:

    .build-children(@type; @rest...) {
      .selector(@type);
      // The recusion step is in `.x-template-helper`
    }

    // This mixin was "assumed" above but is defined here.
    // It is used in the `.selector()` mixin.
    .x-template-helper(@type) {
      .template(@type);
      // Recurse!
      .build-children(@all-types...);
    }

In order to prevent an infinite loop, we define a `@max-depth` and add a `@depth` param to the mixins:

    .build-children(0; ...) { } // base case
    .build-children(@depth; @type; @rest...) {
      .selector(@type);
      .build-children(@depth; @rest...);
    }

    .x-template-helper(@type) {
      .template(@type);
      .build-children((@depth - 1); @all-types...);
    }

Now, when `.build-children(2; @base-types...)` is called, we get the following:

    figure {
      border: 1px solid blue;       // Recall figures have a blue border
      figcaption {
        color: green;               // Recall figure captions are green
      }
      figure {
        border: 1px solid yellow;   // Recall subfigures have a
                                    // yellow border
      }
    }

Which is the desired result.


## Custom classes

So far, we've ignored the `@kind` and `@part` parameters to mixins.
In this section we can add them back in.

The `@part` parameter describes which part of the book the content is in. Some examples are `preface`, `chapter`, or `appendix`. The `@kind` parameter describes the custom class on a piece of content. These are specific to a book (`.how-to` for physics or `.timeline` for a history book).

To handle the `@part` (`preface`, `chapter`, `appendix`, etc) we merely need to add a `@part` parameter to `.build-children()`.

To handle the `@kind` parameter we need to let the skeleton-generation code know what are all the possible classes (`@kind`) for a given type (`figure`, `example`, `section`, etc).

To do this, we add a "function" mixin to the `#content` namespace which will "return" a list of classes for each type.
Because these are specific to each book they are defined as a `#content.kinds(@type)` mixin that "returns" by setting a `@return` variable.

For example, we could have several classes on a figure: `.full-width`and `.half-width`.

The expected CSS would be:

    figure.full-width { width: 100%; }
    figure.half-width { width: 50%;  }

To define these for the skeleton-generator and the add styling in the slots, it would look like:

    #content {
      .kinds(figure) {
        // all the possible classes on a `figure` for this book:
        @return: 'full-width', 'half-width';
      }

      #figure {
        .style('full-width') { width: 100%; }
        .style('half-width') { width: 50%; }
      }
    }

Then, we can add a `@kind` parameter to the definition of `.selector(@type)` and change the corresponding definitions of `.build-children()` and `.x-template-helper()`.

## Custom classes in `@contexts...`

So far, the `@contexts...` arguments only contain a list of types.
For example, styling any `exercise` inside an `example` inside a `chapter` is done like this:

    #content {
      #exercise {
        .style(any; chapter; example) { ... }
      }
    }

To style an `exercise` inside a `.how-to` example we need a bit more information; namely the class `.how-to`. To do this, we can use the `#content>.kinds(@type)` mixin defined above when permuting.

It looks something like:

    .x-helper-base-types(@depth; @contexts; @type; @rest...) {
      .selector(@type; @depth; any; @contexts...);

      // Loop for any custom `@kind`s
      #content>.kinds(@type);
      @kinds: @return;
      .x-helper-custom-types(@depth; @contexts; @type; @kinds...);

      // recurse
      .x-helper-base-types(@depth; @contexts; @rest...);
    }

Now, we can style a figure based on:

- the class
- the context `@type`s
- the context `@kind`s (classes)


## Optimizing

Checking all possible combinations of `@type`s is computation and memory intensive.
Fortunately, we can short-cut several of these.
For example, a `figure` can never contain a `section` or an `example`

Instead of using a global `@all-types` for recursion, we can recurse based on valid children by defining a mixin that "returns" the valid children.

This would look like:

    #skeleton {
      #content {
        .children(figure) { @return: figure, table; } //style subfigures
                                                      //and tables
                                                      //inside a figure
        // Defines the "root" children for a piece of content
        .children(none) { @return: table, figure, exercise, example; }
      }
    }

This mixin is used in `.x-helper-base-types` to restrict the list of children that are permuted.

Now, we can _quickly_ style a `figure` based on:

- the class
- the context `@type`s
- the context `@kind`s (classes)
