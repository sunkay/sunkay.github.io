---
layout: post
title:  "Meteor autoform and check audit errors (audit-argument-checks) "
date:   2015-02-24 8:45:00
categories: meteor
---

While using aldeed:Autoform and in  Meteor.methods, I was using the check argument, but was getting the following  error and stack trace

20150224-21:44:11.402(-5)? Exception while invoking method 'submitPost' Error: Did not check() all arguments during call to 'submitPost'
I20150224-21:44:11.402(-5)?   at [object Object]._.extend.throwUnlessAllArgumentsHaveBeenChecked (/Users/sunil/meteor/meteor-boiler/.meteor/local/build/programs/server/packages/check.js:375:13)

Here is my original meteor method, which threw this error....
'
Meteor.methods({
	submitPost: function(post){
		check(post, {
			title: String,
			url: String
		});
		console.log("in submit post: "+post);
	}
`
The issue is that autoform sends in 3 different arguments to Meteor method. We need to check all the arguments for the check to pass successfully....

Changing to this should work:
'
Meteor.methods({
	submitPost: function(post, modifier, docID){
		check(post, {
			title: String,
			url: String
		});
		check(modifier, Match.Any);
		check(docId, Match.Any);
		console.log("in submit post: "+post);
	}
`
- Resources:
   - [audit argument checks package][check]
   - [Meteor meets Malroy][emily]
   - [Meteor Security Resources][security-resources]
   - [Browser Policy][browser-policy]



[check]: https://atmospherejs.com/meteor/audit-argument-checks
[emily]: http://www.slideshare.net/emilystark/meteor-meets-mallory
[security-resources]: http://security-resources.meteor.com
[browser-policy]: https://atmospherejs.com/meteor/browser-policy

