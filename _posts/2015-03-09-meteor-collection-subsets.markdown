---
layout: post
title:  "Creating meteor collections that are subsets of the larger dataset"
date:   2015-03-09 8:45:00
categories: meteor
---

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

{% highlight javascript %}
Router:
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
