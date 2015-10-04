---
layout: post
title:  "ES6 importing a name vs a module"
date:   2015-10-04 7:45:00
categories: react, ES6, import, module, Link, react-router
---

In ES6 a module can export multiple things by prefixing the keyword export or one thing by using Default.

{% highlight javascript %}
//lib.js
export function_1(){

}
export function_2(){

}
{% endhighlight javascript %}

You can import either the entire module or using its named exports.
{% highlight javascript %}

// importing the entire module
import math from 'lib'
math.function_1();

// importing named module
import {function_1} from 'lib'
function_1();
{% endhighlight javascript %}

The difference is knowing how to use this correctly and reading the module documentation to do the imports correctly. Most times, the module documentation is not entirely clear and you might get into errors that might take you for a spin.

Here is a react-router error that I recently faced in using the <Link to="/" /> functionality.

***
Warning: Failed propType: Invalid prop `children` supplied to `Router`. Check the render method of `Header`.
browser.js:50 Warning: Location "/" did not match any routes
***

Below is the code before and after on how to fix these types of issues.

Importing an entire module causes an issue, as expected
--------------------------------
{% highlight javascript %}
import Link from 'react-router'

export default class Header extends React.Component {
  render(){
    return (
      <nav className="navbar navbar-default header">
        <div className="container-fluid">
          <Link to="/" className="navbar-brand">
            mDiary
          </Link>
          <ul className="nav navbar-nav navbar-right">
            <li>
              <a href=""> Test1 </a>
            </li>
            <li>
              <a href=""> Test2 </a>
            </li>
            <li>
              <a href=""> Test3 </a>
            </li>
          </ul>
        </div>
      </nav>
    );
  }
}

{% endhighlight javascript %}

Fix: Importing just the Link named method from the react-router module
---------------------------------
{% highlight javascript %}
import React from 'react';
import Link from 'react-router'

export default class Header extends React.Component {
  render(){
    return (
      <nav className="navbar navbar-default header">
        <div className="container-fluid">
          <Link to="/" className="navbar-brand">
            mDiary
          </Link>
          <ul className="nav navbar-nav navbar-right">
            <li>
              <a href=""> Test1 </a>
            </li>
            <li>
              <a href=""> Test2 </a>
            </li>
            <li>
              <a href=""> Test3 </a>
            </li>
          </ul>
        </div>
      </nav>
    );
  }
}
{% endhighlight javascript %}
