---
layout: post
title: CSS Coverage for Javascript Unit Tests
---

There are many CSS coverage projects but none plug **directly into JS unit tests**, instrumenting at the same time as the JavaScript coverage.

Assuming you have BlanketJS and Mocha tests, just grab **[css-coverage.js](https://github.com/philschatz/css-coverage.js)** and add the following into the test harness HTML file :

    <link rel="stylesheet/coverage" href="path/to/file.less" />
    <script src="css-coverage.js"></script>

## Less.js + Mocha + BlanketJS

The coverage script uses the `less.tree` AST to parse all the selectors and the `debugInfo` data to get line numbers. It adds a `testdone` hook to Mocha that runs all the selectors on the page. Then, the line number and coverage counts are given to BlanketJS so LESS/CSS files are included in the coverage report.

## Example Screenshot


[![CSS Coverage Demo](https://f.cloud.github.com/assets/253202/2317474/4856dbea-a34e-11e3-92ae-70f53672cb93.png)](/css-coverage.js/test/mocha-demo/)

Check out the [Mocha + BlanketJS demo](/css-coverage.js/test/mocha-demo/) and the [github repo](https://github.com/philschatz/css-coverage.js) for more!

## Other Features

- Loading LESS files using RequireJS is supported with 0 additional work
- Instrumented less files that import other files are automatically instrumented
