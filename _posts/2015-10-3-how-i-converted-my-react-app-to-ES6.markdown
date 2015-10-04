---
layout: post
title:  "How I converted my simple react app to ES6 using babelify and gulp"
date:   2015-09-07 8:45:00
categories: react, ES6, babel, gulp, babelify
---

Here is a repo which implements a simple IMGUR browser client in react. It uses [react router](https://github.com/sunkay/react-flux-imgur). It does not utilize the newest javascript ES6 standard. Writing and converting a simple react app to ES6 allowed me to learn ES6 and in the process allowed me to figure out a way to change my gulp build settings to use the [babel transpiler](https://www.npmjs.com/package/babelify) for ES6.

Here is a step by step tut to convert a standard browserify app to ES6.

Step 1: Clone [Original non-ES6 react repo](https://github.com/sunkay/mdiary/tree/v0.1)
--------------------------------
{% highlight javascript %}
var React = require('react');
<div>
var Hello = React.createClass({
  render: function() {
    return <h1 className="red">
      Hello mDiary!
    </h1>
  }
</div>
});

var element = React.createElement(Hello, {});
React.render(element, document.querySelector('.container'));
{% endhighlight javascript %}
