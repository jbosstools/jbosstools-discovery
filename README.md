To test a site built w/ Jenkins:

0. Run a job, such as https://jenkins.mw.lab.eng.bos.redhat.com/hudson/view/DevStudio/view/DevStudio_Trunk/job/devstudio.discovery_trunk/

1. Install Eclipse (eg., Eclipe 4.3 Kepler JEE bundle)

2. Launch Eclipse:

{code}
  eclipse -vmargs -Djboss.discovery.directory.url=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/jbosstools-directory.xml
{code}

3. Install JBoss Community Central feature from   http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/

4. Restart. Review Central's Software/Update tab. Choose connectors/feature groups to install.

--------------------------------


To build a site locally, you must pass in the following commandline arguments (target platform URL is calculated from parent pom's TARGET_PLATFORM_VERSION):

mvn clean install -f jbosstools/org.jboss.tools.central.discovery/pom.xml \
-DUPDATE_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/trunk/ \
-DEXTRAS_SITE=http://download.jboss.org/jbosstools/updates/kepler/extras/

or

mvn clean install -f jbosstools/org.jboss.tools.central.discovery/pom.xml \
-DUPDATE_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/4.1.kepler/ \
-DEXTRAS_SITE=http://download.jboss.org/jbosstools/updates/kepler/extras/4.30.1/

or

mvn clean install -f jbdevstudio/com.jboss.jbds.central.discovery/pom.xml \
-DUPDATE_SITE=http://www.qa.jboss.com/binaries/RHDS/builds/staging/devstudio.product_trunk/all/ \
-DEXTRAS_SITE=https://devstudio.jboss.com/updates/7.0-staging/extras/

or

mvn clean install -f jbdevstudio/com.jboss.jbds.central.discovery/pom.xml \
-DUPDATE_SITE=http://www.qa.jboss.com/binaries/RHDS/builds/staging/devstudio.product_70/all/ \
-DEXTRAS_SITE=https://devstudio.jboss.com/updates/7.0-development/extras/4.30.1/

You can also point at an existing Central site, if you're not trying to test your locally-built site, using one of these:

-DCENTRAL_SITE=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/
-DCENTRAL_SITE=http://download.jboss.org/jbosstools/discovery/nightly/core/4.1.kepler/
-DCENTRAL_SITE=https://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/trunk/
-DCENTRAL_SITE=https://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/4.1.kepler/

--------------------------------


To test a local site:

0. Run a local web server, so you can access the directory.xml via an http:// URL. You have many options for this. Here's one:

  su
  cd /tmp; wget -nc https://raw.github.com/elonen/nanohttpd/master/NanoHTTPD.java
  # assuming sources checked out into /home/user/devstudio_trunk/
  javac NanoHTTPD.java; java NanoHTTPD -d /home/user/devstudio_trunk/product/ -p 8080

1. Install Eclipse (eg., Eclipe 4.3 Kepler JEE bundle)

2. Launch Eclipse:

{code}
  eclipse     -vmargs -Djboss.discovery.directory.url=http://localhost:8080/discovery/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/jbosstools-directory.xml
    or
  jbdevstudio -vmargs -Djboss.discovery.directory.url=http://localhost:8080/discovery/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/devstudio-directory.xml
{code}

If you did not pass in a CENTRAL_SITE value above to regenerate the plugin.xml w/ a new value, and want to override the value set in plugin.xml via commandline flag, do this:

{code}
  eclipse     -vmargs -Djboss.discovery.directory.url=http://localhost:8080/discovery/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/jbosstools-directory.xml -Djboss.discovery.site.url=http://localhost:8080/discovery/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/
    or
  jbdevstudio -vmargs -Djboss.discovery.directory.url=http://localhost:8080/discovery/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/devstudio-directory.xml -Djboss.discovery.site.url=http://localhost:8080/discovery/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/
{code}

3. Install JBoss Community Central feature from                http://localhost:8080/discovery/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/
    or
   Install JBoss Developer Studio (Core Features) feature from http://localhost:8080/discovery/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/

4. Restart. Review Central's Software/Update tab. Choose connectors/feature groups to install.
