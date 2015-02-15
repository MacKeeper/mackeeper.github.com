---
layout: post
title: "Houston: we lost a table"
description: ""
category: 
tags: []
---
{% include JB/setup %}

For the last 4 months, we have been busy developing an application to manage sports league: [maligue.ca](https://www.maligue.ca/). We started dev with sails, waterline and mongodb. We then switched to postgres just before go live and the transition was smooth.

We are not using waterline anymore for two reasons:

1)
However, 1 week after the beginning of the beta testing, we lost all data in one table. After several hours of investigation, we found out that there is (was) a bug in waterline that generated a delete statement without where clause: https://github.com/balderdashy/waterline/issues/812
I can understand and live with the fact that there could be bugs in such a young open source library. I opened the issue which pointed to the source of the problem (didn't had a patch but pointed to the exact line of the issue) and 2 days later the issue was closed. However, this issue was discovered 2 weeks before and the only answer by the maintainer was "Yikes!". 

I was disappointed by the lack of responsiveness from the maintainers. I would have expected the following:
* Investigation and release ASAP
* Notification to the sails maintainer so they update the dependency and release a new version. BTW, the latest sails 0.10.x release still depend on the bugged waterline version.
* Notification on the user mailing that a major issue was found and that update is required asap.

Nothing of that happened... so we decided to drop waterline.

2)
One reason we switched from mongodb to postgres is that we required an ACID transactional database. The switch to postgres went smooth... until we realized that there is no transaction support in waterline. I have read hacks here and there but it is not properly supported. We will be processing payments soon and we need to be rock solid on what data is persisted and when.


We are in the process of rewriting the whole model layer using bookshelfjs.org / knexjs.org.