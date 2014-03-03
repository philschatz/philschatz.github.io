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
It turns out, quite a bit. If each book is a repository whose contents is an _Unzipped EPUB_ then amazing things are possible.

> Book Publishing = GitHub Repo + GitHub Pages + Download as Zip + Service Hooks

- the `master` branch is the published version of the book
- tags are various versions of a book
- `gh-pages` branch can be used as a book reader (Website)
- "Download as Zip" is the EPUB version of the book
- The editor can "save" via the GitHub API
- Travis-CI can be used to generate a PDF (and "push" to AWS or some other place)

So, I took our editor and made it save EPUB files to GitHub and viola! a book editor using GitHub!

- the [WYSIWYG Book Editor](https://github.com/oerpub/github-book-editor) and [Demo](http://oerpub.github.io/github-book-editor)
- the [PDF-CI server](https://github.com/philschatz/pdf-ci) and server that [generates PDFs on commit](http://pdf.oerpub.org)
- example of the [book reader](http://philschatz.com/epub-anatomy/reader/) using `gh-pages` and [epub.js](https://github.com/futurepress/epub.js)
