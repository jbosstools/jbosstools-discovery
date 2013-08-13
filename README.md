# The JBoss Tools Discovery project

## Summary

The _JBoss Tools Discovery_ project provides a mechanism for building a plugin jar, directory.xml file, and a composite update site used by [JBoss Central](https://github.com/jbosstools/jbosstools-central).

## Install

 To use a Discovery site, you need to first install JBoss Central, which is part of [JBoss Tools](http://jboss.org/tools). Central will then connect to a Discovery site via the default (or other) directory.xml file, which will link to the plugin jar, which will in turn list a catalogue of available updates, installable from a composite site.

## Use

Both the URL for the directory.xml and the URL used within the plugin (to which the directory.xml refers) can be overridden via the commandline when launching Eclipse (or JBoss Developer Studio). Via commandline (or inside your eclipse.ini / jbdevstudio.ini file), you can specify these arguments:

    eclipse -vmargs \
      -Djboss.discovery.directory.url=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/jbosstools-directory.xml
      -Djboss.discovery.site.url=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/

  or

    jbdevstudio -vmargs \
      -Djboss.discovery.directory.url=https://devstudio.jboss.com/updates/7.0-development/devstudio-directory.xml
      -Djboss.discovery.site.url=https://devstudio.jboss.com/updates/7.0-development/central/core/

## Get the code

The easiest way to get started with the code is to [create your own fork](http://help.github.com/forking/), 
and then clone your fork:

    $ git clone git@github.com:<you>/jbosstools-discovery.git
    $ cd jbosstools-discovery
    $ git remote add upstream git://github.com/jbosstools/jbosstools-discovery.git
	
At any time, you can pull changes from the upstream and merge them onto your master:

    $ git checkout master               # switches to the 'master' branch
    $ git pull upstream master          # fetches all 'upstream' changes and merges 'upstream/master' onto your 'master' branch
    $ git push origin                   # pushes all the updates to your fork, which should be in-sync with 'upstream'

The general idea is to keep your 'master' branch in-sync with the
'upstream/master'.

## Building JBoss Tools Discovery

To build _JBoss Tools Discovery_ requires specific versions of Java and
Maven. Also, there is some Maven setup. The [How to Build JBoss Tools with Maven 3](https://community.jboss.org/wiki/HowToBuildJBossToolsWithMaven3)
document will guide you through that setup.

This command will run the build:

    $ mvn clean install

If you just want to check if things compiles/builds you can run:

    $ mvn clean install -DskipTest=true

But *do not* push changes without having the new and existing unit tests pass!
 
## Contribute fixes and features

_JBoss Tools Discovery_ is open source, and we welcome anybody that wants to
participate and contribute!

If you want to fix a bug or make any changes, please log an issue in
the [JBoss Tools JIRA](https://issues.jboss.org/browse/JBIDE)
describing the bug or new feature and give it a component type of
`build`. Then we highly recommend making the changes on a
topic branch named with the JIRA issue number. For example, this
command creates a branch for the JBIDE-1234 issue:

	$ git checkout -b jbide-1234

After you're happy with your changes and a full build (with unit
tests) runs successfully, commit your changes on your topic branch
(with good comments). Then it's time to check for any recent changes
that were made in the official repository:

	$ git checkout master               # switches to the 'master' branch
	$ git pull upstream master          # fetches all 'upstream' changes and merges 'upstream/master' onto your 'master' branch
	$ git checkout jbide-1234           # switches to your topic branch
	$ git rebase master                 # reapplies your changes on top of the latest in master
	                                      (i.e., the latest from master will be the new base for your changes)

If the pull grabbed a lot of changes, you should rerun your build with
tests enabled to make sure your changes are still good.

You can then push your topic branch and its changes into your public fork repository:

	$ git push origin jbide-1234         # pushes your topic branch into your public fork of JBoss Tools Discovery

And then [generate a pull-request](http://help.github.com/pull-requests/) where we can
review the proposed changes, comment on them, discuss them with you,
and if everything is good merge the changes right into the official
repository.
