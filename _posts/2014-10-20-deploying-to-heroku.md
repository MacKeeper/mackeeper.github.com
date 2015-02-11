---
layout: post
title: "Deploying to Heroku"
description: ""
category: 
tags: []
---
{% include JB/setup %}

### Deploying a sails app to Heroku

Deploying to heroku was a [fairly straightforward process](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction) but there are a couple of things to know.

When you push your code to heroku, it will simply call `npm start` in the root directory. By default, this will essentially do the same thing as `sails lift`. This will trigger the grunt build which in turn download client side js dependencies using bower and compile LESS into CSS. This process is asynchronous and can take several seconds. This means that everytime your application is awaken from sleep, it will trigger that process. The problem is that express will serve some files while other are still being generated. In the case of a single page app, the first user after a sleep will get a page without CSS and most client side js library won't be available. 

Also, this process is memory intensive especially if you are running a production build and you activated the concatenation/minification of javascript/css. You might bust your dyno memory limit and it will simply not deploy. I took some time to figure out why development was deploying just fine but not the production setup. The solution: install the [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt.git) buildpack and add the following to your `Gruntfile.js`:

    // for heroku-buildpack-nodejs-grunt
    grunt.registerTask('heroku:development', 'build');
    grunt.registerTask('heroku:production', 'prod');

### Add-ons

We tried to keep cost as low as possible but we still required a couple of heroku add-ons. Here is what we used.

#### 1 web dyno & 1 worker dyno - 34.50$

[As recommended in the heroku documentation](https://devcenter.heroku.com/articles/background-jobs-queueing) we designed the application so we do a little as possible on the web request. We do all the heavy lifting on a worker process using a [queueing system](https://devcenter.heroku.com/articles/asynchronous-web-worker-model-using-rabbitmq-in-node). For example, we send emails and we do big SQL query for batch processing in the worked process.

Note that even the smallest heroku dynos has 4 CPU. This means that you can use [node-cluster](https://devcenter.heroku.com/articles/node-cluster) to create multiple workers inside one dyno. 

One thing you need to know when using heroku is that the [application will be put in sleep mode](https://blog.heroku.com/archives/2013/6/20/app_sleeping_on_heroku) when not used for an hour. It will wake up automatically when a request is made but this is not instantaneous. We initially thought that activating one worker dyno would prevent the application from sleeping. The docs says "Apps that have more than 1 web dyno running never go to sleep and worker dynos (or other process types) are never put to sleep.". The key here is __1 web dyno__. This means that unless you add another web dyno (another 35$ ish/month) or you mange to hit your web worker at least once per hour, your app will sleep.

#### Heroku Postgres - 50$/month

We started with the `Hobby Dev/Basic` plan but we had to upgrade to the `Standard 0` plan. The problem with the introduction plans is the connection limit. When sails setup the database, it creates a lot of parallel connections to read and update the database schema. This maxed out the number of connections and made the process crash.

You should also add [pgbackups](https://addons.heroku.com/pgbackups) addon. It's free and it might save you... at least, it saved us when [something truncated an entire table]({% post_url 2015-01-31-houston-we-lost-a-table %}).

#### Redis Cloud - 0$/month

The free plan was enough since we only persist user sessions in redis. The next plan is only 10$ and would cover our needs to a while. We could also use [client side session](https://hacks.mozilla.org/2012/12/using-secure-client-side-sessions-to-build-simple-and-scalable-node-js-applications-a-node-js-holiday-season-part-3/) to remove the dependency on redis. There is also [a node adapter](https://github.com/mozilla/node-client-sessions) but it's not integrated with sails and the redis integration works so well that it was not worth changing.
 
#### RabbitMQ Bigwig - 0$/month

We started with [CloudAMQP](https://addons.heroku.com/cloudamqp) but switched to [RabbitMQ Bigwig](https://addons.heroku.com/rabbitmq-bigwig).

#### SSL - 20$/month

We handle user login and we will process credit card transactions so https is a requirement. Heroku has [a great guide](https://devcenter.heroku.com/articles/ssl-endpoint) on how to setup that. We purchased our ssl certificate with godaddy (I know...) and they generated two `.crt` files. You will need to [combine them](http://www.joshwright.com/tips/setup-a-godaddy-ssl-certificate-on-heroku) using `cat secure.mysite.com.crt gd_bundle.crt > combined.crt` and then `heroku certs:add combined.crt server.key`.
 
We tried to bypass this add-on and install the certificate straight into node/express/sails. The problem is that heroku firewall everything except port 80. 

### But... this is not a perfect solution

#### No SQL logs

Not being able to get the full query log when debugging a database issue is a big problem. We were lucky enough to be able to reproduce the issue locally but otherwise, we would have been in a dead end.

#### Application sleeps

As we explained, the application will sleep unless you add another web dyno. I think that paying for a worker dyno should prevent the application from sleeping.

#### Not that cheap

104.50$ a month is not much, but when you have 0$ income it's a significant amount of money. To reduce hosting cost, [we tried AWS]({% post_url 2015-02-10-deploying-to-elastic-beanstalk %}).
