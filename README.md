jasmine-node-jquery-matchers in JavaScript
======

jQuery matchers for Jasmine for usage under Node.js environment.
Forked from [rosenfeld's CoffeeScript version](https://github.com/rosenfeld/jasmine-node-jquery-matchers). Since Mr. Rosenfeld doesn't want PRs for a JavaScript version on his project,
I'm putting this up for those who don't want the CoffeeScript dependency.

The only difference between this and Mr. Rosenfeld's version is that his is in CoffeeScript
and mine is compiled to JavaScript (with appropriate changes made to the package.json file).
Version numbers are also the same.

The matchers were extracted from [jasmine-jquery](https://github.com/velesin/jasmine-jquery).

Go check for the available selectors documentation there. Besides those ones, the following
matchers were added to this package: toBeCssHidden, toBeCssVisible, toBeAllHidden, toBeAllVisible,
toBeAllCssHidden, toBeAllCssVisible, toHaveAttrs and toHaveCss.

Differences from jasmine-jquery
------

It seems that the ":hidden" and ":visible" jQuery selectors don't work
correctly under jsdom yet if you're using some CSS class with the 'display'
attribute set (eg.: _.hidden { display: none }_). CSS support needs to be improved in jsdom.

So, as a workaround, toBeCssHidden and toBeCssVisible will test for the
presence/absense of the class provided by its sole argument or the one set in
options.hiddenClass ("hidden" by default).

Installation
------

    npm install git://github.com/rosenfeld/jasmine-node-jquery-matchers

Usage
------

    // Adapted from an example in the jsdom documentation.
    var jsdom = require('jsdom'), jqm = require('jasmine-node-jquery-matchers'),
        already_run = false, $

    jsdom.env('<div id="dialog" class="hidden">My dialog content</div><p>Visible</p>', ['http://code.jquery.com/jquery.min.js'],
      function(errors, window) {
        jqm.options.jQuery = $ = window.$
        jqm.options.hiddenClass = 'hidden'
        jasmine.asyncSpecDone()
      });

    beforeEach(function() { already_run = already_run || jasmine.asyncSpecWait() || true })
    beforeEach(function() { this.addMatchers(jqm.matchers) })

    describe("After jQuery is loaded", function() {
      it("#dialog should be hidden", function() {
        expect($('#dialog')).toBeCssHidden()
      })

      it("p should be visible", function() {
        expect($('p')).toBeVisible()
        $('p').hide()
        expect($('p')).toBeHidden()
      })
    })

Since the matchers were written in CoffeeScript you'll need to call jasmine-node with _--coffee_ option set.

If you want to know how I test my client code, take a look at
[this article](http://rosenfeld.heroku.com/en/articles/programming/2011-10-05-testing-javascript-with-node-jasmine-and-sinon).

Dependencies
------

Since the matchers were written in CoffeeScript as anything else I write for
a JavaScript environment, this package also depend on CoffeeScript.

Of course it would be easy to convert it to a JavaScript only code, but really: who
isn't using CoffeeScript yet?

FAQ
------

q: Why isn't this package published under NPM yet?

a: Because I didn't use all those matchers yet and I'm not sure if they work
as no specs were written for this package yet.

q: Why should I use this untested code?

a: You don't need to. But the code is probably smaller than this README, so you
should really take a look at it and check if it would help you by yourself.