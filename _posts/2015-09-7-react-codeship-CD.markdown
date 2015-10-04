---
layout: post
title:  "Continuous Integration and deployment a React Meteor app using Codeship and webhooks"
date:   2015-09-07 8:45:00
categories: meteor, codeship, react, webhooks
---
Continuous Integration and Continuous deployment, once it is setup allows us to focus on writing software instead of worrying about all the tasks for deployment everytime we need to release. It also helps in deploying code continuosly into production and also should be able to provide tooling and mechanisms to revert back builds auto-magically.

I have been looking for modern tooling and mechanisms which will allow CI/CD.

Here is a very good [article] which covers various Tools that are popular out there today.

The following workflow is what I am trying to put together and hopefully document it along the way. I will take a simple approach for now... Deploy the meteor app into the meteor.com server instead of into modulus or digital ocean.

- github [webhooks]
- [codeship]
- [meteor]


Step 1: Sign up at codeship.com
--------------------------------
  - sign up using your github.com Account
  - follow the initial project setup.
  - Hook codeship to your repository
  - Provide test commands

Step 2: Create a codeship badge
-------------------------------
To show a codeship build status badge, you need to add the markdown syntax to your README.md
  - Go to project settings > notifications
  - Scroll to the bottom and add the markdown given there to your README
  - Voila everytime you push changes, your tests will run

Step 3: Meteor env setup commands at codeship
----------------------
curl -o meteor_install_script.sh https://install.meteor.com/
chmod +x meteor_install_script.sh
sed -i "s/type sudo >\/dev\/null 2>&1/\ false /g" meteor_install_script.sh
./meteor_install_script.sh
export PATH=$HOME/.meteor:$PATH
export VELOCITY_DEBUG=1

Step 4: Testing
---------------------------------
VELOCITY_CI=1 meteor --test

Step 5: Set deployment tasks
---------------------------------
Here we will setup the environment variables and deploy script, which will deploy to meteor.com once the tests succeed.

Add these environment variables into codeship
  - METEOR_EMAIL
  - METEOR_PASSWORD
  - METEOR_TARGET

THe following is the deploy script:

{% highlight javascript %}
  expect -c "set timeout 60; spawn meteor deploy $METEOR_TARGET; expect "Email:" { send $METEOR_EMAIL\r; expect eof }; expect "Password:" { send $METEOR_PASSWORD\r; expect eof }"

{% endhighlight javascript %}

STEP 6: Notifications
----------------------


[cucumber]: https://velocity.readme.io/v1.0/docs/getting-started-with-cucumber
[article]: http://blog.thinkful.com/post/52651178399/under-the-hood-at-thinkful-continuous-integration
[codeship]: https://www.codeship.io/
[webhooks]: http://www.codeaffine.com/2014/10/06/codeship-continuous-integration/
