---
layout: post
title: Openstax Textbooks Reader on GitHub using Kramdown
tags: books
---

Over the weekend I converted the entire Anatomy book from our new HTML format to [kramdown](http://kramdown.gettalong.org) (a Markdown variant that allows classes) and tossed it on GitHub.

Since [gh-pages](http://pages.github.com) automatically converts kramdown files to HTML using [jekyll](http://jekyllrb.com), I also created a minimalist [book reader](https://github.com/philschatz/book-viewer) and linked to it in the book.

Click the screenshot to see [*the reader in action*](http://philschatz.com/anatomy-book/)!

[![image](https://cloud.githubusercontent.com/assets/253202/3496567/d862235a-05e3-11e4-876e-80bdcf516938.png)](http://philschatz.com/anatomy-book/)

# Features

- unobtrusive design
- big next/prev buttons
- reading progress bar
- keyboard left/right buttons work
- Table of Contents shows pages you have read
- spinner shows when page is loading
- just 300 lines of code
- works on any book hosted on *cnx.org* too! [Examples of all the Openstax College books books](http://mountainbunker.org/~schatz/ed/simple-webview/)
- search button reuses the sidebar (mock for now)

# Book Reader Links

- [reader code](https://github.com/philschatz/book-viewer) (it's in `./dist` because I'm lazy)
- [Anatomy & Physiology Reader](http://philschatz.com/anatomy-book/)
- [Openstax College books using the **same** reader code](http://mountainbunker.org/~schatz/ed/simple-webview/) (loads archive.cnx.org)

---

# GitHub.com bonus features

By using a Markdown variant instead of HTML, there are several things GitHub provides for free.

- repo pages will render instead of just showing the markup
- links between pages *just work*
- diffs and Pull Requests render and add changebars/colors to make the changes clearer

## Clickable Table of Contents

![image](https://cloud.githubusercontent.com/assets/253202/3496418/69b29f3a-05e2-11e4-8b72-de53ad718207.png)

**Note:** `{: ...}` is kramdown's way of adding classes

[The markup](https://github.com/philschatz/anatomy-book/blob/gh-pages/SUMMARY.md) (I moved intro modules into the chapter title):


    ---
    layout: page
    ---

    1.  {: .preface} [Preface](contents/m46844.md)
    2.  {: .part} Unit 1: Levels of Organization
        1.  {: .chapter} [An Introduction to the Body](contents/m45981.md)
            1. [Overview of Anatomy and Physiology](contents/m45983.md)
            2. [Structural Organization of the ...](contents/m45985.md)
            3. [Functions of Human Life](contents/m45986.md)

        2.  {: .chapter} [The Chemical Level of ...](contents/m45996.md)
            1. [Elements and Atoms: The Building ...](contents/m45998.md)
            2. [Chemical Bonds](contents/m46000.md)
            3. [Chemical Reactions](contents/m46004.md)

## Rendering in normal GitHub.com

The markdown files render on GitHub.com repo pages ([see example page on GitHub](https://github.com/philschatz/anatomy-book/blob/gh-pages/contents/m46377.md))

![image](https://cloud.githubusercontent.com/assets/253202/3496428/7f327c7c-05e2-11e4-9c90-6a4dde496886.png)

and the [raw kramdown](https://raw.githubusercontent.com/philschatz/anatomy-book/gh-pages/contents/m46377.md):


    ---
    title: "Anatomy of Selected Synovial Joints"
    layout: page
    ---

    <div data-type="abstract" markdown="1">
    By the end of this section, you will be able to:
    * Describe the bones that articulate together to form selected synovial
      joints
    * Discuss the movements available at each joint
    * Describe the structures that support and prevent excess movements at
      each joint

    </div>

    # Articulations of the Vertebral Column

    In addition to being held together by the intervertebral discs, adjacent
    vertebrae also articulate with each other at synovial joints formed
    between the superior and inferior articular processes called <span
    data-type="term">zygapophysial joints</span> (facet joints) (see
    [link text](/m46383#fig-ch09_01_02)). These are
    plane joints that provide for only limited motions between the
    vertebrae. The orientation of the articular processes at these joints
    varies in different regions of the vertebral column and serves to
    determine the types of motions available in each vertebral region. The
    cervical and lumbar regions have the greatest ranges of motions.

    ![Figure alt text](../resource/fig1.jpg "Figure caption"){: data-title="Figure Title" .half-page :}


### Rendered Changes

GitHub will also render the changes of kramdown files instead of showing a diff ([see example of deleting a chapter](https://github.com/philschatz/anatomy-book/commit/bd695b8c50bbdfccc4d892e521b7f8b48d1b55ba?short_path=b4f3573#diff-b4f3573b2f24d5af026c33acf52ff716))

![image](https://cloud.githubusercontent.com/assets/253202/3496449/b512862a-05e2-11e4-8264-19ed006cfa77.png)


# Notes

- Internal links do not work on the OSC cnx.org books because I ran out of time and used `<base href="archive.cnx.org">`
- The reader works even if files are stored in places other than `/contents` (thanks to URI.js)
- kramdown does not support `<figure>` (unless you HTML escape it) so I added attributes to the `<img>` tag (for the title) and JS converts them to a `<figure>`
- Jekyll requires that all MD that is converted to HTML must have a YAML header
- I remove autogenerated ids on paragraphs and lists so the kramdown is cleaner (`id="fs-id*"`)
- exercises do not style quite the same as webview because I ran out of time (webview wraps some elements)
