---
layout: post
title: Midwest.io 2014
date: 2014-07-16 20:00
comments: false
---

Here’s a recap of my experience at [Midwest.io](http://www.midwest.io): an eclectic collection of talks covering the latest trends, best practices, and research in the field of computing.

Conversation archive / slide refs: [Twitter feed](https://twitter.com/search?f=realtime&q=midwestio) (videos will be posted later)

There were many great talks, I highly recommend catching some (or all) of the videos!


#### Mission Critical Innovation

Dr. Jeff Norris Gave a visually compelling story of innovation and what it means to him. This talk was more a TED style, but worth seeing particularly because of his visual story telling. It’s a bit difficult to explain, but very-much worth seeing as the augmented reality presentation style alone is technically compelling.

![488693406190026752](/images/2014-07-16-Midwest_io/BsgwD_JCMAAt-jA.jpg)

“Reflect with me: We launched the tallest building in Florida into space!”


#### Adding Static Type Checking to a Dynamic Language (Julia)

Leah Hanson gave a really interesting explanation of time and space complexity on a simple type inference in a loop as well as nice debugging tools available in the Julia language. This was a particularly enlightening take on dynamic language performance pitfalls that aren’t necessarily readily obvious in usage.

*  http://julialang.org
*  Notable deep-dive https://github.com/JuliaLang/julia/issues/5927


#### Simplifying Big Data with Apache Crunch

Micah Whitacre shared how using Apache Crunch can help with better defining MapReduce pipelines; with the goal to make them easier to write, more testable, and efficient. It’s worth taking a glance at his breakdown of an example workflow and mapping the components with Crunch.


#### Keeping Your Edge by Deploying Faster

Hudl shared their deployment changes that have resulted in faster deploys and greater application stability. A couple of great takeaways were: start doing some research when you’re feeling lost; this ultimately resulted in their use of many Netflix components (using them as a guide). Importance of key performance indicators (KPIs) to benchmark themselves with the changes they implemented (the largest of which was the number of deploys per day and the number of hot-fixes). Also the structure of teams that map to service oriented components, which allowed them to lean on using components both technically and culturally.

*  [Netflix/Hystrix](https://github.com/Netflix/Hystrix) - Control of failure and latency in distributed environments
   *  [hudl/Mjolnir](https://github.com/hudl/Mjolnir)
*  Routing: Eureka metadata routes (spark plug, nginx; not public - yet)


#### Erlang, or How I Learned to Stop Worrying and Let Things Fail

Basho / Riak’s John Daily: “We talk about reusable code, but we almost never talk about rebootable code. ... Failure management shouldn't be an add-on.” Erlang is designed for failure, speed, and has very small processes. “Network transparency: the machine boundary is an ugly place… Async”

Erlang resources: https://gist.github.com/macintux/6349828


#### MythBashers: Adventures in Overlooked Technologies

Avdi Grimm, of Ruby Rogues and Ruby Tapas, gave an overview of diving into a Mythbusters style attack on the hypothesis: “is `bash` web scale?”. Obviously quite a bit of humor and somewhat of a wild-goose-chase in technology; but, the takeaway: “Outlier knowledge - comes from asking outlier questions”. Also the discovery of some new commands, for example: `coproc`, `stdbuf`, `mitmproxy`.

![488798532481343488](/images/2014-07-16-Midwest_io/BsiPrLpIMAAXJ7a.jpg)


#### Robots, JavaScript, and Drones: Welcome to the Hardware Revolution

Julia Grace from Tindie gave a nice overview and opinions of the state of hobbyist hardware. Particularly noting: “2014 is the 50th anniversary of BASIC... There's an analog to hardware *right now*”, i.e. hardware is accessible, usable, and manipulatable by the layman.

![489053720286863360](/images/2014-07-16-Midwest_io/Bsl3xEIIcAAdgkK.jpg)


#### Rules as a Control Structure

Ryan Brush gave a great architecture vs. engineering analogy of blueprint:floor-plan::code:(projection) flow-chart. Noting that we can provide more “live” documentation that assists stakeholders without having to be experts in the underlying components.


#### Data science / Building a Production Machine Learning Infrastructure

Josh Wills shared some experiences of working with machine learning at Google and his opinions of what it takes to have an operationally useful environment. Good guiding principle: “any same thing, but faster is always good”.

Prerequisite for industrial machine learning:

*  Logging
*  Monitoring (to see the change in KPIs)
*  Feature flags (to tune to control KPIs)


Industry examples:

*  [Airbnb](http://nerds.airbnb.com/architecting-machine-learning-system-risk)
*  [Conjecture at Etsy](http://codeascraft.com/2014/06/18/conjecture-scalable-machine-learning-in-hadoop-with-scalding)

Themes:

*  Simple key value pairs
*  Operational lang vs analytical lang (e.g. SciPy vs. Java)

![489084562543476736](/images/2014-07-16-Midwest_io/BsmT0QvCcAMemUX.jpg)


#### Time-series data with Apache Cassandra

Patrick McFadin gave an entertaining and technical overview of storing time-series data in Cassandra. [Time series database requirements](http://www.xaprb.com/blog/2014/06/08/time-series-database-requirements) - Baron Schwartz (of High Perf. MySQL) - good basis for needs. Interesting tidbit: Cassandras use of TimeUUID (UUID sortable by time) - v1 UUID.


#### Chef / Packing It In: Images, Containers, and Config Management

Michael Goetz overviewed a workflow building VMs from scratch and using containers (with Docker) to speed up deployment and get a handle on deployed/configured environments. Obviously using Chef as the configuration management tool, but also noting the physical/time constants like always building from scratch can be very time consuming. This also introduces the concept of convergence (coming into alignment) vs congruence (rebuilding from scratch).

Hallway-track takeaways:

*  Chef-zero should replace chef-solo (local mode server)
*  Chef-metal will eventually have PXE boot


#### Mobile Ad Hoc Networks (MANET) Are Coming, But We Aren't Ready

Justin Collins shared his views and opinions in the exploration of MANETs and what technologies we’d need to support the next “killer-app” in this space. This was an interesting introduction into technologies that are emerging and necessary, particular in mobile, computing; also notable: peer-to-peer and Delay-tolerant networking (DTN).

*  https://github.com/presidentbeef/melon
*  http://tools.ietf.org/html/rfc2501
*  Color audience commentary: FireChat
