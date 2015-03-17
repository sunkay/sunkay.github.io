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
----------------------------------
{% highlight javascript %}
File: /server/fixtures.js

  // create 500 fake users
  for(var i=0; i<500; i++){
    var usr = Fake.user({
        fields['username', 'email', 'profile.name']
      });

    Accounts.createUser({
      username: usr.username,
      email : usr.email,
      password : usr.username+'123',
      profile  : {
          //publicly visible fields like firstname goes here
          name: usr.profile.name,
      }
    });  
  }

{% endhighlight javascript %}

Step 2: Do meteor reset & run meteor to create this data
--------------------------------------------------------
  - cmd-line> meteor reset
  - cmd-line> meteor
  - cmd-line> meteor mongo
  - cmd-line> db.users.find()
    + make sure the user data is there...
  - Go to http://localhost:3000
  - Login as admin user: username: admin@y.com, [admin123]
  - Click on Admin - Users link in the sidebar 
  - It should list all 500 users without pagination

Step 3: Create a route controller [UsersListController] 
--------------------------------------------------------

{% highlight javascript %}
In lib/user-routes.js

- Redirect to the controller for typical paths
Router.route('/users', function(){
  this.redirect('/users/paginate/10');
}); 
Router.route('/users/paginate', function(){
  this.redirect('/users/paginate/10');
}); 

Router.route('/users/paginate/:usersLimit?', {
  name: "userList",
}); 

UserListController = RouteController.extend({
  template: 'userList',
  increment: 10,
  usersLimit: function(){
    return parseInt(this.params.usersLimit) || this.increment;
  },
  findOptions: function(){
    return {sort: {createdAt: -1}, limit: this.usersLimit()}
  },
  subscriptions: function(){
    this.usersSub =  Meteor.subscribe('allusers', this.findOptions());
  },
  data: function(){
    var nextPath = this.route.path({usersLimit: this.usersLimit() + this.increment});
    return {
      ready: this.usersSub.ready,
      nextPath: nextPath
    }
  }
});

{% endhighlight javascript %}

The route controllers for pagination follow a pattern that can be replicated for other subscriptions. 

Step 4: Add the 'Load More' block to the template
--------------------------------------------------------
{% raw %}
<template name="userList">
  {{#contentHeader heading="Users" currentPage="Users"}}
  <ul class="todo-list">
    {{#each allusers}}
    {{> userItem}}
    {{/each}}

    {{#if nextPath}}
      <div class="callout callout-info">
        <h3>
          <a class="load-more btn-block" href="{{nextPath}}">Load more</a>        
        </h3>
      </div>
    {{/if}}
  </ul>
  {{/contentHeader}}
</template>
{% endraw %}

[boiler]: http://sunkay.github.io/meteor-boiler/
[roadmap]: https://trello.com/b/grrlZ9pd/meteor-boilerplate
