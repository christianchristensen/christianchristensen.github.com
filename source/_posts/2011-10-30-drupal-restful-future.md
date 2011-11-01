---
layout: post
title: "Drupal's RESTful future"
date: 2011-10-30 20:41
comments: true
categories: drupal
---

TL;DR: [services](http://drupal.org/project/services) and [feeds](http://drupal.org/project/feeds) rock! We need to change the way we look at the server - web-servers are really build-servers.

-------

#### Intro

  Lately at [AllPlayers.com](http://www.allplayers.com) I have been spending a lot of time working on API's for our site and spending some time making Drupal talk *web*. Luckily, this is something that is being thought about a lot in the community (see: [WSCCI](http://groups.drupal.org/wscci), [services.module](http://drupal.org/project/services)); but, there are some interesting growing pains:
 <!-- more -->

  The *connection* to Drupal: There are a variety of ways to [connect to external sites](http://drupal.org/project/modules?filters=tid%3A52) and obviously "the way the web should work", but I get the explicit feeling that many of types of connections are "toys". As a site administrator we built out a DLAMP (Drupal, Linux, Apache, MySQL, PHP) install because we are *serious* about building a web-app! In our engineering efforts, we are probably only slightly interested in handing over our data to 3rd party services, so likely our next interest is in taking on the data for ourselves (which is no small matter).

#### Get the data

  So, we've committed ourselves to "[big data](http://en.wikipedia.org/wiki/Big_data)"; great - how do we do it?! Honestly - look towards to the industry leaders; short of a major engineering effort (which perhaps you should be looking at hadoop, hbase, and map-reduce), we are here for Drupal - and that's gonna make this fast and powerful: [feeds](http://drupal.org/project/feeds)! Target an endpoint, upload a file, or configure the plugin an do what you want, then parse that data with a processor (e.g. XPath, QueryPath, CSV mapping, or build your own). It's a little like magic! (if you don't believe me, maybe a [demo](http://developmentseed.org/blog/2009/dec/15/importing-and-aggregating-stuff-feeds) could convince you. Now let's get a little more technical...)

##### Oh the pain...

  You've got the data but, was that all there was to it? Mostly... but, because we are good researchers we note something about feeds requests: [they're pretty good](http://drupalcode.org/project/feeds.git/blob/refs/heads/master:/libraries/http_request.inc#l86), but there turns out to be a variety of different ways to do essentially the same thing:

*  [`drupal_http_request`](http://api.drupal.org/api/drupal/includes--common.inc/function/drupal_http_request/7)
*  [World Bank API](http://drupalcode.org/project/wbapi.git/blob/refs/heads/master:/wbapi.request.inc#l164) (depends on `drupal_http_request`)
*  [Acquia Connector](http://drupalcode.org/project/acquia_connector.git/blob/refs/heads/7.x-1.x:/acquia_agent/acquia_agent.module#l553) which uses [XML-RPC](http://api.drupal.org/api/drupal/includes--common.inc/function/xmlrpc/7) (depends on `drupal_http_request`)
*  [Pantheon System API](https://github.com/pantheon-systems/pantheon7/blob/master/modules/pantheon/pantheon_api/pantheon_api.inc#L113-168)

  Some observations: *the Drupal way* seems to be `drupal_http_request` but, there is also a distinct lack of PEAR dependencies like [HTTP_Request](http://pear.php.net/package/HTTP_Request2); a nice place to ruminate.

#### Expose your data

  Data acquired! But we want to share (easy): [fields](http://drupal.org/documentation/modules/field-ui) and [views](http://drupal.org/project/views). However, as a respecting netizen you want to share semantically! Drupal already comes [feature packed for the web](http://www.lullabot.com/articles/how-does-rdf-work-drupal-7), but we also want to share through API's. Luckily contrib space has this "solved" with [services](http://drupal.org/project/services), where we can expose our data through a variety of [servers](http://drupalcode.org/project/services.git/tree/refs/heads/7.x-3.x:/servers) and varieties of [formats](http://drupalcode.org/project/services.git/blob/refs/heads/7.x-3.x:/servers/rest_server/includes/rest_server.views.inc#l25).

  Thus we can do something like [this](https://gist.github.com/affc9864487bb1b9c918): `GET` http://drupal6-services/services/plist/node/1.json and expect

    {
      "changed": "1286592762",
      "language": "",
      "uid": "0",
      "type": "story",
      "status": "0",
      "nid": "1",
      "created": "1286592762"
      ...
    }

amazingly powerful! (it is also worth noting that the services endpoint can pull from views as well!)

##### Oh the pain...

  The story so far has gotten us everything we need *very quickly*, but as consumers of [many](https://dev.twitter.com/docs/api) [public](http://developers.facebook.com/docs/reference/rest) [APIs](http://www.twilio.com/docs/api/rest/) we quickly note that what we are leaking is Drupal through exposing endpoints. This is actually fairly useful to developers of Drupal, but is probably pretty useless to someone consuming your *product*, therefore we need to take [configure (or theme)](http://drupalcode.org/project/services.git/blob/refs/heads/7.x-3.x:/services.services.api.php) the exposed services.

  Additionally, given a glimpse of [web services future of Drupal](http://www.garfieldtech.com/blog/web-services-initiative) this seems particularly pertinent! [WSCCI](http://groups.drupal.org/wscci) (Web Services Context Core Initiative) is the holding place for the *high performance REST server*.

#### Data and the front-end interaction

  I probably don't share very much, but I have a [love affair](http://twitter.com/#!/imetchrischris/status/116723898380328960) with javascript; and given the amazing [HTML5 features](http://slides.html5rocks.com) and great [javascript libraries](http://microjs.com) ([jQuery](http://jquery.com/) is just the tip of the iceberg - or the first taste depending on how you look at it) - there is a bright future in the browser! So bright in fact: I am of the opinion that "web-pages" should be delivered as apps! There are a lot of great tools to facilitate the concept of a client side app, often examples of this are seen in the [Chrome web store](https://chrome.google.com/webstore) (some neat technologies include the [cache-manifest](http://www.w3.org/TR/html5/offline.html)).

  This leaves me wondering: where exactly does the theme layer live? Particularly, as in a traditional sense, the theme layer seems to live "in the browser", which means that any processing done server side to render CSS, JS, and/or HTML would be in the *delivery of an application* - the version of which the client is requesting to be delivered by the server.

