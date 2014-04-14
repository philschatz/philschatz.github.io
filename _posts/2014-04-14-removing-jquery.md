---
layout: post
title: Removing jQuery, and adding Promises
tags: javascript
---

This weekend I decided to remove jQuery from 2 of my libraries: [css-polyfills.js](/css-polyfills.js/) and [octokit.js](https://github.com/philschatz/octokit.js).

It was motivated by requests from octokit users and by the horrible performance of [css-polyfills.js](/css-polyfills.js/) on large textbooks (took ~1.5 hours).

## Refactoring [Polyfills](/css-polyfills.js/)

I started with [you-might-not-need-jquery](http://youmightnotneedjquery.com/) and made incremental progress through the codebase. Initially my hope was to decrease the time by 50% but the refactor was only providing marginal improvements, until I hit the following line of code:

    $('#' + id)

This uses Sizzle to find an element by id. As soon as I turned it into the following...

    document.getElementById(id)

**BAM!** the time dropped from ~1.5 hours down to 4.5 _minutes_.

**TODO:** Iterating over live `NodeList` objects (use `_.toArray()`)


## Refactoring [octokit.js](https://github.com/philschatz/octokit.js)

This one was a bit easier.

`octokit.js` uses 2 features in jQuery: `jQuery.ajax()` and `jQuery.Deferred`.

ECMAScript 6 has native support for `Promise` (and the more fun generators) and it seems the `XMLHTTPRequest` object is not going away any time soon.

Fortunately, it does not do any DOM manipulation so refactoring this only required changing a few isolated spots in the code.

There were some benefits in making the change as well as drawbacks.

As a reward for being almost a decade ahead of its time, jQuery promises are almost, but not quote the same as ECMAScript Promises. They are constructed differently, have different names for functions, and have more features than the native Promises.

For example:

- waiting on all Promises to complete in jQuery requires calling `jQuery.when`
- jQuery promises have `.notify()` and `.progress()` methods
- distinguish between waiting on a promise and extending one (`.done()` vs `.then()`)
- allow multiple arguments to `.resolve()`

For octokit this would have been a nice way to provide paginated results as a second argument.

