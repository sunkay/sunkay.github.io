---
layout: post
title:  "Meteor server side debugging"
date:   2015-03-05 8:45:00
categories: meteor
---

Came across the "meteor debug" command that was introduced in the Meteor 0.9.4 release. This is so far not documented in the official docs but it is a pretty useful tool. 

> meteor debug

Just type in "meteor debug" on the command line and it will attach to the debug port. You can then connecto to that port to start debugging server side code using node-inspector. 

There is no reason to set environment variables, start node-inspector within another terminal window etc....

