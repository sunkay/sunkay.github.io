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

  Roles.addUsersToRoles(admin_id, 'admin');

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

} else {
{% endhighlight javascript %}

Step 2: Do meteor reset & run meteor to create this data
--------------------------------------------------------
  - cmd-line> meteor reset
  - cmd-line> meteor

Step 3: Do meteor reset & run meteor to create this data
--------------------------------------------------------

Step 4: Do meteor reset & run meteor to create this data
--------------------------------------------------------


Step 5: Do meteor reset & run meteor to create this data
--------------------------------------------------------


Step 6: Do meteor reset & run meteor to create this data
--------------------------------------------------------

- Resources:
   - [audit argument checks package][check]
   - [Meteor meets Malroy][emily]
   - [Meteor Security Resources][security-resources]
   - [Browser Policy][browser-policy]


[boiler]: http://sunkay.github.io/meteor-boiler/
[roadmap]: https://trello.com/b/grrlZ9pd/meteor-boilerplate
