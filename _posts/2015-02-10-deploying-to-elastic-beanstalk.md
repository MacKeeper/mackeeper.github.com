---
layout: post
title: "Deploying to Elastic Beanstalk"
description: ""
category: 
tags: []
---
{% include JB/setup %}

### Elastic Beanstalk: Pain err... Platform as as Service




You can simply pipe the output to a log file and tail it during deployment:

commands:
  01-install-git:
    command: "yum install -y git &>> /tmp/deploy.log"
  02-install-nodejs-npm:
    command: "yum install -y --enablerepo=epel nodejs npm &>> /tmp/deploy.log"
  03-install-grunt:
    command: "npm install -g grunt-cli &>> /tmp/deploy.log"
  04-install-coffee:
    command: "npm install -g coffee-script &>> /tmp/deploy.log"
  05-install-bower:
    command: "npm install -g bower &>> /tmp/deploy.log"
container_commands:
  01_grunt:
    command: "export PATH=$PATH; grunt prod &>> /tmp/deploy.log"





It's in the /tmp/deployment/application folder during deployment and the moved to /var/app/current afterward.

In case you search them, the node logs are in /var/log/nodejs/nodejs.log and the application will bind to 8081 no matter what PORT environment variable you set in the Environment Variables in the console.