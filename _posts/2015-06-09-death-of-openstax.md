---
layout: post
title: The Death of Openstax?
tags: books
---

[OpenStax](https://openstaxcollege.org) isn't going anywhere; we have a ton of high-quality content and are revolutionizing textbook publishing for the benefit of students. But here are my thoughts on how it could be made a bit more open (and cheaper to boot!).

# Problem

As background, openstax books contain a few pieces:

1. an editor for creating a part of a book (Aloha) and organizing parts of a book into a Table of Contents
2. rules for attribution (who authored the book)
3. a way to convert the book into various formats (ePUB and PDF)
4. a way to read the book online for free
5. a way to mix-and-match and create a new book
6. Ideally, openstax will have a way to allow others to suggest edits to a book

# Solution

## Non-tech-savvy

1. Use GitHub to store the book content ([book viewer](http://philschatz.com/books), [source for openstax books](https://github.com/search?p=1&q=user%3Aphilschatz+Openstax), and [blog post](../../../2014/07/07/tiny-book-reader))
2. Use a [browser editor](https://github.com/oerpub/github-bookeditor) to edit the book
3. Use a little server to automatically create PDFs and ePubs ([travis-ci](https://travis-ci.org) or [philschatz/pdf-ci](https://github.com/philschatz/pdf-ci))
4. Support attribution automatically ([example](https://github.com/Connexions/webview/graphs/contributors))
5. Support "Suggested Edits" to content automatically via [GitHub's "Pull Request"](https://help.github.com/articles/using-pull-requests/) ([example](https://github.com/philschatz/anatomy-book/commit/bd695b8c50bbdfccc4d892e521b7f8b48d1b55ba?short_path=b4f3573#diff-b4f3573b2f24d5af026c33acf52ff716))
6. Support "Derived Copies" of content automatically via [GitHub's "Fork"](https://help.github.com/articles/using-pull-requests/)
7. Development of code and content using GitHub Issues via [philschatz/gh-board](https://github.com/philschatz/gh-board)

## Tech-savvy

1. All book content is stored directly in GitHub (see [sources](https://github.com/search?p=1&q=user%3Aphilschatz+Openstax) for examples)
2. Replace the editor with [oerpub/github-book-editor](https://github.com/oerpub/github-bookeditor) which saves **directly** to GitHub
3. Replace the web view with [Autogenerated Sites using GitHub](https://pages.github.com) (see [philschatz.com/books](http://philschatz.com/books) and the [various book repositories](https://github.com/search?p=1&q=user%3Aphilschatz+Openstax) )
4. Replace PDF (and ePub) generation with [travis-ci](https://travis-ci.org) or [philschatz/pdf-ci](https://github.com/philschatz/pdf-ci) (Every time GitHub updates, generate a new PDF)
5. Support Derived copies using [GitHub's "Fork"](https://help.github.com/articles/using-pull-requests/)
6. Support "Suggested Edits" (which openstax used to have) via [GitHub's Pull Requests](https://help.github.com/articles/using-pull-requests/)
7. Support a diff of "Suggested Edits" via GitHub's Markdown diff view (see [blog post on books in GitHub](http://philschatz.com/2014/07/07/tiny-book-reader/))
8. Support software (and content) development using something on top of GitHub Issues like huboard or ideally [philschatz/gh-board](https://github.com/philschatz/gh-board) (no server/subscription required)

# Free Stuff!

1. No server costs! (except PDF generation which can be minimized)
2. autogenerated PDFs for every edition (see GitHub Tags and Releases)
3. easy contributions from the entire world
  - [travis-ci](https://travis-ci.org) can make sure the Markdown is well-formed
4. human-readable changes
5. Issue tracking for content in each book
6. Revisions for each book
7. More reusability/remixability than you could imagine

# The Kink

There is a "kink" in this process which I'd be happy to elaborate on:

1. Each book needs to be a separate repository (cannot "easily" combine multiple books into 1)
2. Need to learn MarkDown (specifically kramdown) unless the editor is "smart enough"


That's how I would "kill" OpenStax with minimal effort and stay true to the openness that has helped [OpenStax](https://openstaxcollege.org) thrive.

For more projects check out [my repositories](https://github.com/philschatz?tab=repositories)
