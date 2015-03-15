---
layout: post
title:  "How to create pagination with Meteor Boiler - tutorial"
date:   2015-03-15 8:45:00
categories: meteor, meteor-boiler, meteor boilerplate, pagination
---

[Meteor Boilerplate meteor-boiler][boiler] purpose is to allow new and experienced developers to start applications quickly. It has borrowed heavily from various meteor resources, books and articles to come up with patterns that can be utilized effectively. The [roadmap] is @trello to see where it is headed...

This post discusses how to create pagination for large collections. It follows the pattern described in the Discover Meteor book. Extend it as you see fit. 

Lets go through a step by step walkthrough in creating pagination for users collection, which could get fairly large. 

- Pre-Requisites:
  + git clone https://github.com/sunkay/meteor-boiler.git
  + mv meteor-boiler Your-Project-Name
  + cd Your-Project-Name
  + meteor 
    +  in browser go to localhost:3000
    + Check if all the tests have successfully passed

Step 1: Create a large set of data

This is a pattern where you want to have multiple subscriptions for a single collection. In a normal meteor application, only the signed in user information is available on the client. What if we want to create an admin page which lists a subset of users in the DB? 
  We can create a local client collection, which is a subset of the data on the server. 

We use the _publishCursor method to do this. 

Server:
{% highlight javascript %}
Meteor.publish('allusers', function(){
  var sub = this;

  var allusersCursor = Meteor.users.find({});
  Mongo.Collection._publishCursor(allusersCursor, sub, 'allusers');

  sub.ready();

});
{% endhighlight javascript %}

Client:
{% highlight javascript %}

// client side only users collection
UserList = new Mongo.Collection('allusers');

{% endhighlight javascript %}

Router:
{% highlight javascript %}
// user list
Router.route('users', {
  name: "userList",
  waitOn: function(){
    return Meteor.subscribe('allusers');
  }
});

Template.userList.helpers({
  allusers: function(){
    return UserList.find();
  }
});
{% endhighlight javascript %}

- Resources:
   - [audit argument checks package][check]
   - [Meteor meets Malroy][emily]
   - [Meteor Security Resources][security-resources]
   - [Browser Policy][browser-policy]


[boiler]: http://sunkay.github.io/meteor-boiler/
[roadmap]: https://trello.com/b/grrlZ9pd/meteor-boilerplate
