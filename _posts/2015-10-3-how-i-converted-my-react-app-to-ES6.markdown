---
layout: post
title:  "How I converted my simple react app to ES6 using babelify and gulp"
date:   2015-09-07 8:45:00
categories: react, ES6, babel, gulp, babelify
---

Here is a [repo](https://github.com/sunkay/react-flux-imgur) which implements a simple IMGUR browser client in react. It uses [react router](https://github.com/sunkay/react-flux-imgur). It does not utilize the newest javascript ES6 standard. Writing and converting a simple react app to ES6 allowed me to learn ES6 and in the process allowed me to figure out a way to change my gulp build settings to use the [babel transpiler](https://www.npmjs.com/package/babelify) for ES6.

Non-ES6 simple react class
--------------------------------
{% highlight javascript %}

var React = require('react');
var ReactRouter = require('react-router');
var HashHistory = require('react-router/lib/hashhistory');
var Router = ReactRouter.Router;
var Route = ReactRouter.Route;

var Main = require('./components/main');
var Topic = require('./components/topic');
var ImageDetail = require('./components/image-detail');

module.exports = (
  <Router history={new HashHistory}>
    <Route path="/" component={Main}>
      <Route path="topics/:id" component={Topic} />
      <Route path="images/:id" component={ImageDetail} />
    </Route>
  </Router>
);

{% endhighlight javascript %}

ES6 version of a similar react-router class
---------------------------------
{% highlight javascript %}

import React from 'react';
import {Router, Route} from 'react-router';
import createHashHistory from 'history/lib/createHashHistory';

import Main from './components/main';
import Test from './components/test';


export default(
    <Router history={createHashHistory()}>
      <Route path="/" component={Main}>
        <Route path="test" component={Test} />
      </Route>
    </Router>
);
{% endhighlight javascript %}

Adding babel to Gulp file to transpile ES6 into ES5
----------------------------------------------------
Add babelify npm package using ** npm add babelify --save **
In your gulp file require babelify
use ***transform(babilify)*** in your bundle method. Code given below... A link to the repo where this is working... 

{% highlight javascript %}

***var babelify = require('babelify')***

function bundle() {
  return bundler
    ***.transform(babelify)***
    .bundle()
    .on('error', notify)
    .pipe(source('main.js'))
    .pipe(gulp.dest('./'))
}
{% endhighlight javascript %}

References
---------------------------------------------
[Original non-ES6 react repo](https://github.com/sunkay/mdiary/tree/v0.1)

[ES6-version repo](https://github.com/sunkay/mdiary/tree/develop)
