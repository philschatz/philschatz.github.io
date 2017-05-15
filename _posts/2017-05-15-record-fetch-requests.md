---
layout: post
title: Automatically record HTTPS requests instead of manually creating mock files
tags: javascript, octokit
---

Creating mock HTTP requests seems to be the go-to method for developers.

But wouldn't is be easier if you could just point your tests to a server and record the HTTP responses and then play them back?

Now there is [fetch-vcr](https://github.com/philschatz/fetch-vcr)!

Since [fetch](https://fetch.spec.whatwg.org) API is in browsers, there is an easier way to do this in JavaScript!

[fetch-vcr](https://github.com/philschatz/fetch-vcr) will record and play back those HTTP requests for you.

# How does it work?

Just load the `fetch-vcr` package in your tests and it will proxy any calls to `fetch(url, options)`.

Each HTTP response is saved to the filesystem as a cassette (also known as a fixture).
The cassette contains all of the response headers and the response body.

Depending on the `VCR_MODE` it will do the following:

- `playback`: (default) it will load the recorded cassette of the request instead of talking to the server
- `record`: it will talk to the server and save the response to a local file (cassette)
- `cache`: it will try to load the cassette and if the cassette is not found, it will do the same thing as `record`

Because it saves files to the filesystem, it works slightly differently when used in NodeJS and when used in a browser.

# How does it work in NodeJS?

It's super-simple. When running tests:

1. Make sure your code runs `var fetch = require('fetch-vcr');` instead of `var fetch = require('node-fetch');`
1. Record your cassettes by running `VCR_MODE=record npm test` instead of just `npm test`
1. Optionally, change the directory that cassettes are saved to by running `fetchVCR.configure({fixturePath: '/path/to/folder/'})`

Viola! you have just recorded your HTTP requests.


# How does it work in a browser?

For browser tests it is a little bit more complicated because browsers do not save files to the filesystem.

Fortunately, browsers like PhantomJS or Selenium allow you to send data from the browser using `alert(msg)`. There are other ways if you would rather do it differently.

You can use the steps listed above but will need to do the following additional steps:

1. Pass the `VCR_MODE` environment variable to the browser
1. Replace `fetchVCR.saveFile(rootPath, filename, contents) => Promise` with a function that calls `alert(JSON.stringify([rootPath, filename, contents]))`
1. Parse the alerts and save them to disk using `fs.writeFile(filePath, contents)`
  - **Note:** This code does not run in the browser; it runs in the JS file given to PhantomJS (or Selenium if you are using Selenium)

Check out [fetch-vcr](https://github.com/philschatz/fetch-vcr) for more info.
