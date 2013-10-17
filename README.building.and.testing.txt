To build a site locally, you must pass in the following commandline arguments (target platform URL is calculated from parent pom's TARGET_PLATFORM_VERSION):

  mvn clean install -f jbosstools/org.jboss.tools.central.discovery/pom.xml \
    -DUPDATE_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/trunk/ \
    -DEXTRAS_SITE=http://download.jboss.org/jbosstools/targetplatforms/jbtcentraltarget/kepler/

or

  mvn clean install -f jbosstools/org.jboss.tools.central.discovery/pom.xml \
    -DUPDATE_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/4.1.kepler/ \
    -DEXTRAS_SITE=http://download.jboss.org/jbosstools/targetplatforms/jbtcentraltarget/kepler/

or

  mvn clean install -f jbdevstudio/com.jboss.jbds.central.discovery/pom.xml \
    -DUPDATE_SITE=http://www.qa.jboss.com/binaries/RHDS/builds/staging/devstudio.product_master/all/repo/ \
    -DEXTRAS_SITE=http://download.jboss.org/jbosstools/targetplatforms/jbtcentraltarget/kepler/

or

  mvn clean install -f jbdevstudio/com.jboss.jbds.central.discovery/pom.xml \
    -DUPDATE_SITE=http://www.qa.jboss.com/binaries/RHDS/builds/staging/devstudio.product_71/all/repo/ \
    -DEXTRAS_SITE=http://download.jboss.org/jbosstools/targetplatforms/jbtcentraltarget/kepler/

--------------------------------


To test a local site:

0. Run a local web server, so you can access the directory.xml via an http:// URL. You have many options for this. Here's one:

  su
  cd /tmp; wget -nc https://raw.github.com/elonen/nanohttpd-original/master/NanoHTTPD.java
  # assuming sources checked out into /home/user/jbosstools-discovery/
  javac NanoHTTPD.java; java NanoHTTPD -d /home/user/jbosstools-discovery/ -p 8080


1. Build TP:

  pushd jbtcentraltarget
  BASEDIR=`pwd`
  # Merge changes in new target file to actual target file
  pushd multiple && mvn -U org.jboss.tools.tycho-plugins:target-platform-utils:0.16.0-SNAPSHOT:fix-versions -DtargetFile=jbtcentral-multiple.target && rm -f jbtcentral-multiple.target jbtcentral-multiple.target_update_hints.txt && mv -f jbtcentral-multiple.target_fixedVersion.target jbtcentral-multiple.target && popd
  # Resolve the new 'multiple' target platform and verify it is self-contained by building it
  mvn -U install -DtargetRepositoryUrl=file://${BASEDIR}/multiple/target/jbtcentral-multiple.target.repo/
  popd

2. Build Discovery plugin + site:

  mvn clean install -f jbosstools/org.jboss.tools.central.discovery/pom.xml \
    -DUPDATE_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/4.1.kepler/ \
    -DEXTRAS_SITE=http://localhost:8080/jbtcentraltarget/multiple/target/jbtcentral-multiple.target.repo/

or

  mvn clean install -f jbdevstudio/com.jboss.jbds.central.discovery/pom.xml \
    -DUPDATE_SITE=http://www.qa.jboss.com/binaries/RHDS/builds/staging/devstudio.product_71/all/repo/ \
    -DEXTRAS_SITE=http://localhost:8080/jbtcentraltarget/multiple/target/jbtcentral-multiple.target.repo/

3. Install Eclipse (eg., 4.3.1 Kepler SR1 JEE bundle)

4. Launch Eclipse:

{code}
  eclipse -vmargs \
          -Djboss.discovery.directory.url=http://localhost:8080/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/jbosstools-directory.xml \
          -Djboss.discovery.site.url=http://localhost:8080/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/
    or
  jbdevstudio -vmargs \
          -Djboss.discovery.directory.url=http://localhost:8080/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/devstudio-directory.xml \
          -Djboss.discovery.site.url=http://localhost:8080/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/
{code}

5. Install JBoss Community Central feature from                http://localhost:8080/jbosstools/org.jboss.tools.central.discovery/target/discovery-site/
    or
   Install JBoss Developer Studio (Core Features) feature from http://localhost:8080/jbdevstudio/com.jboss.jbds.central.discovery/target/discovery-site/

6. Restart. Review Central's Software/Update tab. Choose connectors/feature groups to install.

--------------------------------

To test a site built w/ Jenkins:

1. Install Eclipse (eg., 4.3.1 Kepler SR1 JEE bundle)

2. Launch Eclipse:

{code}
  eclipse -vmargs \
          -Djboss.discovery.directory.url=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/jbosstools-directory.xml \
          -Djboss.discovery.site.url=http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/
{code}

3. Install JBoss Community Central feature from   http://download.jboss.org/jbosstools/discovery/nightly/core/trunk/

4. Restart. Review Central > Software/Update tab. Choose connectors/feature groups to install.

5. Or, launch JBDS like this:

{code}
./jbdevstudio -vmargs \
          -Djboss.discovery.directory.url=http://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/trunk/devstudio-directory.xml \
          -Djboss.discovery.site.url=http://www.qa.jboss.com/binaries/RHDS/discovery/nightly/core/trunk/
{code}
