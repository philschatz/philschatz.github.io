---
layout: post
title: HTML+CSS Diffs
---


## CSS Diffs

Our textbooks are frequently hundreds of pages long and use a single CSS file, so making a CSS change can change content in unexpected places.

Again, [CSS Polyfills](/css-polyfills) and a little XSLT file makes this easy.

[CSS-Diff](https://github.com/philschatz/css-diff) will take an HTML and CSS file and produce an HTML file with all the styling "baked" in.
Then, the provided XSLT file can compare 2 "baked" HTML files and inject `<span>` tags whenever the styles differ.


Style Baker code (shortened for brevity):

    class StyleBaker
      rules:
        # Match all rules and squirrel the styles away
        '*': (env, name) ->
          $context.addClass('js-polyfill-styles')
          styles = $context.data('js-polyfill-styles') or {}
          rules = []
          for arg, i in arguments
            continue if i < 2
            rules.push(arg.eval(env).toCSS(env))

          styles[name] ?= style
          $context.data('js-polyfill-styles', styles)

    # Run Polyfills and bake in the styles
    polyfills = new CSSPolyfills(new StyleBaker())
    polyfills.run $root, lessFile, (err, newCSS) ->

      # Bake in the styles.
      console.log('Baking styles...')
      $root.find('.js-polyfill-styles').each (i, el) ->
        $el = $(el)
        style = []
        for ruleName, ruleStr of $el.data('js-polyfill-styles')
          style.push("#{ruleName}:#{ruleStr}; ")

        $el.attr('style', style.join(''))


See the [CSS-Diff](https://github.com/philschatz/css-diff) project to run it from the commandline.
