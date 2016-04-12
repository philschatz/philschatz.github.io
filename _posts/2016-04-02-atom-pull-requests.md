---
layout: post
title: Introducing the atom pull-requests package!
tags: javascript, octokit
---

As a programmer that uses GitHub, Pull Requests are a great way to discuss code but whenever I get feedback on code in a large codebase it is annoying to have to find where that change was. Since I use [atom.io](https://atom.io) as my text editor, I decided to write a plugin that adds a great feature of GitHub (Pull Requests) into my text editor (which also happens to be written by GitHub). And _viola_! **[pull-requests](https://github.com/philschatz/atom-pull-requests)**.

Whenever you check out a branch that has a Pull Request and open it in GitHub, you'll see the GitHub comment directly on the line of code inside atom. And to help you find it, the Tree view on the left will show how many comments are in the directory so you can find the file.

[![screenshot](https://cloud.githubusercontent.com/assets/253202/11326511/82360626-9139-11e5-8466-ed2d356cb0d8.png)](https://atom.io/packages/pull-requests)

Here's a slightly out-of-date screencast showing the whole process (including installing the plugin):

![process](https://cloud.githubusercontent.com/assets/253202/11237087/a3568100-8dab-11e5-8d9d-3bc9cc3dc5af.gif)


# How's it made?

It uses the [octokat.js](https://github.com/philschatz/octokat.js) npm package and the [linter](https://atom.io/packages/linter) atom package. [pull-requests](https://github.com/philschatz/atom-pull-requests) checks if your code is in a git repository (actually, a GitHub one) and then checks if the branch corresponds to a Pull Request in the repository (or the parent repository if this is a forked repository) and pulls out the comments on the changes of a file.

Then, it uses `linter` to add lint messages on the lines of the files. Since `linter` supports HTML, [pull-requests](https://github.com/philschatz/atom-pull-requests) also converts the comment into HTML (complete with emojis) and adds a link to get back to the Pull Request on GitHub so you can continue discussing.


Hope that helps you!
