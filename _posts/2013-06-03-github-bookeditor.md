---
layout: post
title: Building an eBook using GitHub
---

At [Connexions](http://cnx.org) we build textbooks. To do it we have roughly 4 services:

- Published Repository
- Authoring Repository
- Generate exports (EPUB, PDF)
- Website

As someone who uses GitHub and Travis daily, I wondered how much of this can GitHub (and Travis) do for us?

It turns out, quite a bit. If each book is a repository whose contents is an Unzipped EPUB:

- the `master` branch is the published version of the book
- tags are various versions of a book
- `gh-pages` branch can be used as a book reader (Website)
- "Download as Zip" is the EPUB version of the book
- The editor can "save" via the GitHub API
- Travis-CI can be used to generate a PDF (and "push" to AWS or some other place)

So, I took our editor and made it save EPUB files to GitHub and viola! a book editor using GitHub!

- the [Book Editor Demo](http://oerpub.github.io/github-book-editor) for the WYSIWYG editor ([code](https://github.com/oerpub/github-book-editor)).
- the [PDF-CI server](http://pdf.oerpub.org) for generating PDFs on commit ([code](https://github.com/philschatz/pdf-ci))
- example of the [book reader](http://philschatz.com/epub-anatomy/reader/) using `gh-pages` and [epub.js](https://github.com/futurepress/epub.js)
