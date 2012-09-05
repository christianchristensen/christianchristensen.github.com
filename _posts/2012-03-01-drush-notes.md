---
layout: post
title: "Drush tips and tricks + make, dl, .info inject"
date: 2012-03-01 20:00
comments: false
categories: drupal
---

<article class="markdown-body"><p>Did some testing with drush tonight and have some tips:</p>

<p><a href="http://drupal.org/project/drush">http://drupal.org/project/drush</a> project has nice pear install info:
(note: may need to sudo)</p>

<pre><code>pear channel-discover pear.drush.org
pear install drush/drush-5.0.0 # note: there may be some instability, but seems ok
</code></pre>

<p>(note: drush-5.0.0 has the --gitinfofile which injects version into .info)</p>
<!-- more -->

<p><a href="http://drupalcode.org/project/drush.git/blob/refs/heads/master:/README.txt#l79">Drush bash completion</a> is probably a handy tool for sbox CLI: <a href="http://drupalcode.org/project/drush.git/blob/refs/heads/master:/drush.complete.sh">http://drupalcode.org/project/drush.git/blob/refs/heads/master:/drush.complete.sh</a>
namely: sourcing the bash completion script e.g.: <code>. /usr/share/php/drush/drush.complete.sh</code>
(this provides tab completion for drush commands)</p>

<p>Drush5 provides an injection to the .info file; an example use of this would be:
<code>drush dl --package-handler=git_drupalorg --gitinfofile uuid</code>
(note: the --gitinfofile parameter)</p>

<p>(running <strong>.info inject</strong> by default)
Using the drushrc.php (a file to specify specific specific drush options in a config file...) we can ensure that <code>gitinfofile</code> is enabled by default:
<code>$command_specific['pm-download'] = array('gitinfofile' =&gt; TRUE);</code>
(note: unfortunately drush_make doesn't respect the <code>drush dl</code> paradigm ... drush 5 this will be the preferred method)
(note: to get drush to continue reading my rc file I had to do: <code>drush -c ~/.drush/drushrc.php make example.make www</code>)
(tip: to see what is happening with the actual drush commands use debug/verbose: <code>drush --debug --verbose -c ~/.drush/drushrc.php make example.make www</code>)</p>

<p>We can also have specific <strong>drushrc.php</strong> settings <em>per repo</em> to be fancy:
<a href="http://drupalcode.org/project/drush.git/blob/refs/heads/master:/examples/example.drushrc.php#l8">http://drupalcode.org/project/drush.git/blob/refs/heads/master:/examples/example.drushrc.php#l8</a>
and/or
<a href="http://drupalcode.org/project/drush.git/blob/refs/heads/master:/examples/example.drushrc.php#l332">http://drupalcode.org/project/drush.git/blob/refs/heads/master:/examples/example.drushrc.php#l332</a>
(note: I believe this to be a nice way - and the drush recommended way - to organize our alias' and other drush specific settings.)
(note: to accomplish this I believe minimal setup through Puppet could get things in place - this would be for deployed env. - everything else would have a Readme)</p>

<p>&lt;(soon) deprecated&gt;</p>

<p>For <strong>drush_make .info inject</strong>... (or running drush 4.5)</p>

<pre><code>cd ~/.drush/drush_make/
# http://drupal.org/node/1302820
wget http://drupal.org/files/drush_make_inject_info.patch
patch -p1 &lt; drush_make_inject_info.patch
</code></pre>

<p>&lt;/deprecated&gt;</p></article>

