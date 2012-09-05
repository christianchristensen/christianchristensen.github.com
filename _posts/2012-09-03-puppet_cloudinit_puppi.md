---
layout: post
title: Rollin like a pro
date: 2012-09-03 20:00
comments: false
---

I have recently been on a bit of a devops journey in an attempt to optimize for happiness (and some substantial learning). At [my work](https://allplayers.com) we've committed to Puppet for a variety of configuration management tasks; ranging from in office infrastructure, to developer sandboxes, to our production application. Given the recent opportunity to work on a project a bit out of band from our main application(s) I was in a mode to seek out configuration management and deployment on par with process like {heroku, appfog, phpfog, etc…}. It turns out this magic is only a few scripts and some discipline away!


## Enter CloudInit + Puppet Git Receiver + Puppi

### [Puppet Git Receiver](http://brightbox.com/blog/2012/08/29/puppet-git-receiver/)

I believe the most "magical" aspect of any of the PaaS offerings is the ability to push code+infrastructure changes (IMO the best being with git) to a remote server. The Puppet git reciever has to be one of the cleanest and well thought out implementations of what is essentially a fancy post-recieve git hook.

What makes this so great, besides the really clean and well thought out code to deploy reasonably complex servers with Puppet, is the simple deb package install that basically configures a git repo, with hook scripts, and configured users.


### [CloudInit](https://help.ubuntu.com/community/CloudInit)

CloudInit is a nice tool that makes this whole process quite seamless. Basically CloudInit allows you to specify a set of scripts (and Ubuntu specific commands) to run at the VM creation; effectively letting you bootstrap a machine *exactly* the way you want it. (This is a little additional help that traditionally lives as process step to login to a new machine and "get it going")


At this point with CloudInit + Puppet git reciever - you can now have a "blank" server ready to recieve whatever infrastructure defined by Puppet. This leaves your application code, which is addressed by … (read on)


### [Puppi](https://github.com/example42/puppi)

I'm curious if this Puppet module should actually be called Puppet-Deploy, because [that's what it does](http://www.example42.com/?q=Puppi_A_Puppet_module_for_Deployment_Automation)! (along with some additional sysadmin helper tools too) By [specifying a few simple instructions](https://github.com/example42/puppi/blob/master/README.deploy) this module configures and stages everything for your app to run on our accompanying infrastructure.

    class myapp {
      puppi::project::git { "myapp":
        source      => "https://github.com/../myapp.git",
        deploy_root => "/srv/myapp",
      }
    }

Simply run: `puppi deploy myapp` on the target server (noteworthy: this can be scripted a variety of ways: with Puppet, via SSH, etc…).


![Happy Dance](/images/Seinfeld-ToeDance.gif)


## But wait! There's more!

Given that we're already building out a server and deploying our app with super-simple-commands, why not make it easier? Yeah lets do that! Check out [librarian-puppet](https://github.com/rodjek/librarian-puppet) a tool to manage Puppet module dependencies similar to how Bundler, Composer, and the like. This let's us specify a `Puppetfile` in our infrastructure repo that defines all the "common" modules we will be using - making a **very** clear the line between libraries and the "working set" (and avoiding those pesky submodules). Bonus points to letting the [post recieve hooks](https://github.com/brightbox/puppet-git-receiver/pull/11) auto build the Puppetfile! This can lead to some pretty simple [infrastructure+app definitions](https://github.com/christianchristensen/puppet-php-quick-start):

    ├── Puppetfile
    ├── README.md
    ├── manifests
    │   └── site.yml
    └── modules
        ├── imapnote
        │   └── manifests
        │       └── init.pp
        └── mysites
            └── manifests
                └── init.pp


(Brought to you by: Labor Day Weekend Research Group™)
