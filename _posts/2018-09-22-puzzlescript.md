---
layout: post
title: Embeddable and Accessible PuzzleScript Games
tags: puzzlescript
---

I ❤️ Puzzle Games.

> I want kids to play games that don't talk down to them, encourage them to figure out game mechanics on their own, **and** I want non-sighted people to play video games.

Introducing: the [puzzlescript](https://github.com/philschatz/puzzlescript) package.

[![a couple levels of the game Pot Wash Panic](https://user-images.githubusercontent.com/253202/47249708-7a204080-d3dc-11e8-8aee-73179c314a41.gif)](https://github.com/philschatz/puzzlescript)

Many games have puzzles sprinkled in, but I like ones that are distilled into just solving puzzles.

I first realized how engrossing these were while playing [The Witness](http://the-witness.net) with friends. One person would control the player but everyone would be solving the puzzle in their head just by watching. We played together for days.

Then, I tried it with kids. It turns out, a 6-year-old grasped the puzzle concepts in [The Witness](http://the-witness.net) faster than her parents and was _explaining to her parents_ how the puzzles worked.

Then, I started looking for more and stumbled upon [increpare](https://twitter.com/increpare)'s awesome [PuzzleScript](https://www.puzzlescript.net).

Unlike other video games that tend to teach kids to memorize facts (like "Carmen San Diego", "Oregon Trail", or "Math Blaster"), these encourage kids to think critically, and problem-solve in groups.

Specifically, PuzzleScript games are interesting because the whole game is a text file and the levels are typically pretty small. This means it's easy for kids to explore but it also makes the games playable by people that can't see. The reason is that all of the sprites have human-readable names. So we can read those out instead of just showing colored pixels.


## Background

Inspiration came from both the tons of easter-eggs found in software ([Chrome Dino Game](https://www.blog.google/products/chrome/chrome-dino/), [various Microsoft ones](https://en.wikipedia.org/wiki/List_of_Easter_eggs_in_Microsoft_products) and commandline tools) as well as a desire to get more people in general to play video games.

## Goals

- get kids in classrooms playing these games
- get vision-impaired people playing these games
- get 404 pages to have these games when something is broken (like the [Chrome Dino Game](https://www.blog.google/products/chrome/chrome-dino/)). Example: [this website's 404 page](/404)
- get people to play these instead of Sudoku to exercise their brains more


## Try it out!

Games can run embedded in a webpage **or** in a commandline terminal. There are **over 200 games** to choose from!

### Terminal

All you need is [node 4 or higher](https://nodejs.org) and then run the following:

```sh
npm install --global puzzlescript
puzzlescript
```

See [philschatz/puzzlescript](https://github.com/philschatz/puzzlescript#screencaps) for examples.


### Embed in a Webpage

```html
<body>
  <table id="the-game-ui"></table>
  <script src="https://unpkg.com/puzzlescript@3.0.0/lib/webpack-output.js"></script>
  <script>
    const gameSourceString = '...' // Source code for the game
    const table = document.querySelector('#the-game-ui')
    const engine = new PuzzleScript.TableEngine(table)
    engine.setGame(gameSourceString, 0 /*startLevel*/)
    engine.start()
  </script>
</body>
```

Visit the [demo page](/puzzlescript/) to play several games (there are **over 200**), or see [this website's 404 page](/404) for an example of using it in a 404 page.

## For Next Time

I would like to go through more details on how the code is organized. But if something does not work or is not clear, [create an issue](https://github.com/philschatz/puzzlescript/issues).

## More Examples

### Mirror Isles ([original](http://www.draknek.org/games/puzzlescript/mirrors.php))

![mirror-isles](https://user-images.githubusercontent.com/253202/47133542-ce0d1700-d26e-11e8-851f-233d27aaf0b8.gif)

More examples can be found in the [README](https://github.com/philschatz/puzzlescript) or see the [demo page](/puzzlescript/) to play them in a browser.
