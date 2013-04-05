JBoss Central has a "software/update" page which gets its content from a hardcoded URL, set in the org.jboss.tools.central plugin.

The forthcoming (current) milestone release URLs for JBT/JBDS are:

	http://download.jboss.org/jbosstools/discovery/development/kepler/jbosstools-directory.xml
  https://devstudio.jboss.com/updates/7.0-development/devstudio-directory.xml

The stable branch (upcoming milestone) URL are:

	http://download.jboss.org/jbosstools/discovery/nightly/core/4.1.kepler/
  http://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/4.1.kepler/

The unstable trunk URLs are:

	http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/
  http://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/trunk/

To point Eclipse or JBDS at a different site, use:

{code}
  eclipse     -vmargs -Djboss.discovery.directory.url=http://download.jboss.org/jbosstools/discovery/nightly/core/4.1.kepler/jbosstools-directory.xml
    or
  jbdevstudio -vmargs -Djboss.discovery.directory.url=http://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/4.1.kepler/devstudio-directory.xml
{code}

Or, you can add the -Djboss.discovery.directory.url flag to your eclipse.ini (of jbdevstudio.ini) file after the -vmargs line.
