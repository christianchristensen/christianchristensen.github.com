---
layout: post
title: "Drupal Camp Austin"
date: 2011-11-20 20:00
comments: false
categories: drupal
---
[Drupal Camp Austin 2011 (gist)](https://gist.github.com/1392744)

<!-- more -->
-------

<div class="wikistyle"><h2>
<a href="http://2011.drupalcampaustin.org/">Drupal Camp Austin 2011</a> - 11/19/2011</h2>

<h2>Responsive design</h2>

<ul>
<li>mediaqueri.es</li>
<li>Progressive CSS</li>
<li>Feature detection (JS - modernize)</li>
<li>front-end reverse proxy ignorance</li>
<li>polyfill... JS shim that makes ie work</li>
<li>mediaquery polyfills

<ul>
<li>respond.JS</li>
<li>adaptjs</li>
</ul>
</li>
<li>Responsive modules

<ul>
<li>respondjs</li>
<li>responsive_images</li>
<li>responder</li>
</ul>
</li>
<li>script loaders

<ul>
<li>modernizer</li>
<li>yepnope</li>
<li>labjs</li>
</ul>
</li>
<li>themes

<ul>
<li>adaptivetheme</li>
<li>omega</li>
</ul>
</li>
<li>responsive video: fitvidjs</li>
<li>front end performance

<ul>
<li>~97% perceived response time is front</li>
</ul>
</li>
<li>Responsive images, datauri, ...</li>
<li>jdrop.org</li>
<li>Stevesounders.com/mobileperf</li>
<li>Blaze.io/mobile</li>
</ul><h2>Practical Performance Tuning</h2>

<ul>
<li><a href="http://www.opensourcetesting.org/performance.php">http://www.opensourcetesting.org/performance.php</a></li>
<li>Siege <a href="http://www.joedog.org/index/siege-home">http://www.joedog.org/index/siege-home</a> to intentionally crash servers with multiple concurrent requests to server.</li>
<li>XHProf</li>
<li>Disable varnish, memcached, any cache to clearly distinguish performance issues.</li>
</ul><h2>miscellaneous</h2>

<ul>
<li>psr0 module (may not be autoload compatible)</li>
<li>Composer (package management)</li>
<li>drush make version package injection and drupal modules</li>
<li>worker queue - drupal queue with drush in long running mode to pick up jobs (worker pool) - use reddis for queue (in place of beanstalkd for reporting and queryability) </li>
</ul><h2>Pantheon (David Strauss)</h2>

<ul>
<li>Moved to Chef Solo, no more Bcfg2</li>
<li>Nginx + PHP-FPM</li>
<li>Cassandra? based file system for public/private files (distributed + redundant)

<ul>
<li>Valhalla?</li>
<li>Probably going to move to S3 or Rackspace analog for cheaper rates (redundancy on instance storage is very expensive $1/GB x 3)</li>
</ul>
</li>
<li>SASL API communication</li>
<li>Hidden Jenkins job runner</li>
<li>Running Fedora, primarily for system.d.</li>
</ul><h2>Examiner</h2>

<ul>
<li>use mongodb for storage (no MySQL - motivated by trying to avoid cacheing) </li>
<li>mongodb writes are a bear... they have run out of indexes and are in constant tuning mode for performance</li>
<li>interesting scale problem with 1000x usage spikes - autoscale is difficult</li>
<li>use hadoop for (batched) "live jobs" that need to compute complex royalty rules</li>
</ul><h2>Code Smell (Larry Garfield)</h2>

<ul>
<li>and (ie object_validate_and_save)
*

<ul>
<li>drupal_get_form (retrieve, populate a form0</li>
<li>Solr::addDocuments wraps _documentToXML private method, so you can’t see the object XML without adding to the index.</li>
<li>God objects - classes that do too much</li>
</ul>
</li>
<li> parameters or returns of multiple types

<ul>
<li>throw exceptions</li>
<li>return object or FALSE</li>
<li>_registry_check_code() -&gt; constants, strings, variable types

<ul>
<li>Volunteer to cleanup this function</li>
</ul>
</li>
</ul>
</li>
<li>Overly complex code leads to overly complex bugs.

<ul>
<li>comment_node_view !!!</li>
</ul>
</li>
<li>Run-time type identification

<ul>
<li>Changing keys by object type = bad</li>
<li>Polymorphism (procedural)

<ul>
<li>callback</li>
<li>messy</li>
</ul>
</li>
<li>Polymorphism (OO)

<ul>
<li>method, clear</li>
</ul>
</li>
</ul>
</li>
<li>4 Unit Testing (263)

<ul>
<li>SimpleTest in core are not unit tests unless 1 Drupal = unit

<ul>
<li>This is system testing and is valuable, but not unit testing.</li>
</ul>
</li>
<li>DrupalUnitTestCase (~25)

<ul>
<li>We should be doing more of this, but...</li>
<li>Globals, hooks, etc don’t handle it well</li>
<li>If you can’t unit test your code, your code is wrong.</li>
</ul>
</li>
<li>Avoid globals, avoid statics</li>
<li>Dependency injection, construct objects with what they need, don’t call other API midstream.</li>
<li>Minimize singletons, another form a global.  Hard function calls are a form of singleton.</li>
</ul>
</li>
<li>5 Documentation

<ul>
<li>If you can’t document, you don’t know what you are doing.</li>
<li>If you don’t document, another wont know what you are doing.</li>
<li>Bad documentation:

<ul>
<li>string $jail -&gt; what is jail, what is valid, must define</li>
<li>array $settings -&gt; what goes in settings?</li>
</ul>
</li>
<li>Why things aren’t documented</li>
<li>Lazy -&gt; bad</li>
<li>Indifference -&gt; bad</li>
<li>Lack of comprehension

<ul>
<li>That’s OK, document that! (Here be dragons)</li>
</ul>
</li>
<li>Embarrassment

<ul>
<li>That’s OK, document it!  Very useful.</li>
</ul>
</li>
<li>What to document

<ul>
<li>Every function</li>
<li>Every method</li>
<li>Every class</li>
<li>Every object property</li>
<li>Every constant</li>
<li>Every parameter</li>
</ul>
</li>
<li>drupal.org/node/1354</li>
</ul>
</li>
<li>
<p>6 Inappropriate intamacy</p>

<ul>
<li>coupling

<ul>
<li>content coupling</li>
<li>Common</li>
<li>External</li>
<li>Control</li>
<li>Data-structured</li>
<li>?</li>
<li>?</li>
</ul>
</li>
<li>Otherwise difficult to refactor</li>
<li>Solutions

<ul>
<li>Well documented interfaces</li>
</ul>
</li>
<li>7 Purity</li>
<li>Pure functions always returns the same value given the same input.</li>
<li>Side effects

<ul>
<li>Changes global state, cannot be repeated, etc.</li>
</ul>
</li>
<li>Sometimes side effects are the goal

<ul>
<li>ie save() -&gt; that’s fine, but isolate save()</li>
</ul>
</li>
<li>drupal_theme_initialize()

<ul>
<li>Lot’s of side effects</li>
<li>cleaner approach</li>
<li>class Theme -&gt; more testable</li>
</ul>
</li>
<li>8 Good smells</li>
<li>Single purpose</li>
<li>Self-contained</li>
<li>Predictable</li>
<li>Repeatable</li>
<li>Unit testable</li>
<li>Documented</li>
</ul>
</li>
<li><p>aphorisims-api-design</p></li>
<li><p>coding horror -&gt; code smells</p></li>
<li><p>java -&gt; SmellsToRefactoings</p></li>
<li><p>joel spoelsky -&gt; Wrong.html</p></li>
<li><p>signs you should not be coding</p></li>
<li>
<p>TheDailyWTF.com</p>

<ul>
<li>Good code</li>
<li>Queue</li>
<li>DBTNG (biased)</li>
<li>File transfer for module update</li>
<li>PHP Object patterns and practice, it’s a bit old</li>
<li>Sitepoint book coming soon.</li>
<li>Avoid older PHP tutorials</li>
</ul>
</li>
</ul><h2>Project mgmt (from the Examiner)</h2>

<ul>
<li>PERT - from military - provides more insight than gantry</li>
<li>Focus on stakeholder needs rather than wants</li>
<li>last 3 mo. Finally unhacked core</li>
<li>a bad decision is <em>better</em> than indecision on a deadline</li>
<li>a bad client is better than no client at all</li>
<li>process and schedules: client should be just as accountable</li>
<li>communication, mediate and translate, unblock -- calm from chaos</li>
<li>minimize little shiny distractions -- context switching kills!</li>
<li>manhole cover round and heavy - keep it from falling in and stop from easily walking away - for saftey</li>
</ul><h2>Drupal Commerce</h2>

<ul>
<li>ecommerce framework - flexibility about what you can build with it</li>
<li>eurocentres</li>
<li>opendealsapp.com</li>
<li>inline product form... in rsyzmar sandbox</li>
<li>Ajax framework in d7 is really impressive with commerce</li>
<li>zondervan??</li>
<li>realmilkcheese.com</li>
</ul><h2>Profiling with Xhprof</h2>

<ul>
<li><a href="http://msonnabaum.github.com/xhprof-presentation/#1">http://msonnabaum.github.com/xhprof-presentation/#1</a></li>
<li> brianmercer ppa</li>
<li>Module implements should go into pressflow - David Strauss... follow up.</li>
<li>XHProf should run in production on a few requests... </li>
<li>Helps to have non-file backend on multi-node setup.</li>
<li>msonnabaum/XHProfLib</li>
<li>Strongarm patch to fix variable cache performance issue</li>
</ul><h2>Devops bof</h2>

<ul>
<li>git flow</li>
<li>gitalyte?</li>
<li>git tags? Are they locked and never supposed to change?</li>
<li>drush make idea: [patch][104065] = ...</li>
<li>Jenkins: DB backup, Dev to stage to prod jobs</li>
<li>etc keeper - package commit hooks - </li>
<li>backups and restore scripts -- testable in Jenkins</li>
<li>update hook - reads from text file</li>
<li>devel generate - integrations</li>
<li>default content module - phase2.... will be.dropped</li>
<li>queued Node access rebuild</li>
<li>updatedb - revert features in hook_update</li>
<li>drush deploy (like cap deploy) - video in London</li>
<li>git reference cache</li>
<li>camptocamp -- puppet conf -- apache2</li>
<li>Vewee_fun - stevenmerill</li>
<li>fastly</li>
<li>tungsten?? MySQL</li>
</ul></div>

