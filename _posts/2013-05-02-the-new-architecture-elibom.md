---
layout: post
title: The new architecture of elibom.com
---

# The new architecture of elibom.com

In a few days we will launch a new version of [elibom.com](http://www.elibom.com), an SMS Gateway for Latin America that I co-founded a while ago[^1]. From banks notifying unusual transactions to small businesses reminding appointments, we currently have hundreds of customers using our web application and sending millions of messages per month.

In this post I'll explain the technical details behind the new version, which is currently hosted at [beta.elibom.com](http://beta.elibom.com).

## Architecture

The following diagram shows the main components of the new architecture:

![Deployment view of the new architecture](/assets/images/posts/architecture-elibomcom.png)

### Modules
The application is divided in three modules, _web_, _worker_ and _scheduler_. _Web_ modules handle HTTP requests, _worker_ modules execute background jobs and the _scheduler_ module triggers scheduled text messages and periodic tasks (e.g. to generate statistics or clean data). Notice that there are no hard dependencies between them.

_Web_ and _worker_ modules are deployed on multiple [EC2 instances](http://aws.amazon.com/ec2/) (m1.small) while the _scheduler_ module is deployed on a single \[free\] [Heroku dyno](https://devcenter.heroku.com/articles/dynos). The reason to use [Heroku](http://heroku.com/) \-for the _scheduler_ instance\- is that, besides being free, it will monitor and restart the process if it fails.

Notice that there is only one instance of the _scheduler_ module. There is a reason for that. We can't have more than one instance or unexpected things will happen (e.g scheduled messages could be triggered multiple times). Besides, the _scheduler_ module doesn't need to scale because it does little work (albeit an important one).

So, why not having everything on [Heroku](http://heroku.com/)? Well, it turned out that [Heroku dynos](https://devcenter.heroku.com/articles/dynos) were too small for our _web_ and _worker_ processes[^2]. Also, we experienced some latency to other services. I don't know exactly why, [Heroku](http://heroku.com/) is supposed to deploy all the instances on _us-east-1_ but you can't really tell. Anyway, that's still an option we are considering.

### External services

An [Amazon RDS for MySQL](http://aws.amazon.com/rds/mysql/) (Multi-AZ) acts as our main database. We also use [MongoDB](http://mongodb.org/)  (powered by [MongoLab](http://mongolab.com/)) to store things that doesn't quite fit the relational model. [Redis](http://redis.io/) (powered by [OpenRedis](http://openredis.com)) is used to store session information, for inter-module communication and to store volatile data.

We use [Amazon S3](http://aws.amazon.com/s3/) to store files and assets.

Although not shown in the diagram, there is an external service called [Mokai](https://github.com/germanescobar/mokai) that we use to actually deliver text messages to mobile operators. The communication between our web application and [Mokai](https://github.com/germanescobar/mokai) is done through [Cloud AMPQ](http://www.cloudamqp.com/), a messaging solution built on top of [RabbitMQ](http://www.rabbitmq.com/).

## Technologies

### Front End

Our front end is built using HTML5, CSS3 and Javascript. CSS is generated using [LESS](http://lesscss.org/). [Twitter Bootstrap](http://twitter.github.io/bootstrap/) is used for the layout and for some visual components.

[Backbone](http://backbonejs.org/), [Underscore](http://underscorejs.org/) and [JQuery](http://jquery.com/) are the Javascript libraries we use the most. We deliberately decided **not** to build a one-page application so we only use a part of [Backbone](http://backbonejs.org/): the views, and occasionally models and collections.

### Back End

The MVC framework we use is [Jogger](https://github.com/germanescobar/jogger), which brings the best ideas of other frameworks such as [Sinatra](http://www.sinatrarb.com/), [Flask](http://flask.pocoo.org/), and [Express.js](http://expressjs.com/) to the Java language. The views are generated using [Freemarker](http://freemarker.sourceforge.net/), which is a templating engine for Java.

We use [Hibernate](http://www.hibernate.org) as the [ORM](http://en.wikipedia.org/wiki/Object-relational_mapping) and [Hazelcast](http://www.hazelcast.com/) as the Second Level Cache[^3]. [Spring Framework](http://www.springframework.org) glues everything together.

To execute background jobs we use [Jesque](https://github.com/gresrun/jesque), a port of [Resque](https://github.com/defunkt/resque) to the Java language, which is a Redis-backed library for enqueuing and processing background jobs. The _web_ and _scheduler_ modules enqueue jobs that the _worker_ module dequeues and executes asynchronously.

## Deployment and monitoring

Our deployment process is not as automated as I would want it to be, but that's something we are working on. We are using [Cloudbees](http://www.cloudbees.com/) as our CI server but it doesn't publish the artifacts after a successful push. We have to do it manually in two steps.

First, we have to create the distribution files using an instance on EC2 with a script that pulls from our Github repository, builds the Maven project and publish the artifacts to S3.

Then, to actually deploy the changes we have to launch new EC2 instances passing a [User Data](http://www.turnkeylinux.org/blog/ec2-userdata) script that gets executed when the instance starts. The script downloads the distribution file, uncompresses it and starts the application. There are two User Data scripts: one for the _web_ instances and the other for _worker_ instances. Finally, we manually add the new _web_ instances to the load balancer. 

Yeah, lots of improvements to do. However, it allow us to continuously deploy with zero downtime.

Deploying to [Heroku](http://heroku.com/) is a lot easier, we just push the code to [Heroku's](http://heroku.com/) git repo and we are done! Our tests showed that it does have some downtime but because we only have the _scheduler_ module there, it's not a real issue.

Finally, we are using [New Relic](https://newrelic.com/) to monitor our instances, which is expensive but incredibly powerful. 
<br/><br/>

I hope this post has given you valuable information for your current or future projects. The idea was to give you an overview of what is possible and what technologies enable those possibilities. If you have questions or want me to expand on a specific topic just let me know, I'll try to do my best.

[^1]: 10 years ago at the time of this writing!
[^2]: [Heroku](http://heroku.com/) recently launched 2X dynos but we haven't tried those yet!
[^3]: I would love to see a good Second Level Cache implementation using Redis.