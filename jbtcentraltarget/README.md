## Building Target Platforms

To build, you require specific versions of Java and Maven. Also, there is some Maven setup. 
The [How to Build JBoss Tools with Maven 3](https://community.jboss.org/wiki/HowToBuildJBossToolsWithMaven3)
document will guide you through that setup.

This command will run the build:

    $ mvn clean verify

If you want to run the build and fetch source bundles at the same time as other bundles are being resolved, do this:

    $ mvn clean verify -Dmirror-target-to-repo.includeSources=true

If you want to run the build and not fail if there's a problem w/ validation, do this:

    $ mvn clean verify -Dvalidate-target-platform.failOnError=false

If you just want to check if things compiles/builds you can run:

    $ mvn clean verify -DskipTest=true

But *do not* push changes without having the new and existing unit tests pass!
 

## Updating versions of IUs in .target files

When moving from one version of the target to another, the steps are:

0. Bump the target platform versions contained in all 5 pom.xml and 2 *.target files.

1. Update the URLs contained in jbosstools-multiple.target and jbdevstudio-multiple.target (by hand). Check in changes (but do not push to master).

2. Regenerate the IU versions, using <a href="https://github.com/jbosstools/jbosstools-maven-plugins/wiki">org.jboss.tools.tycho-plugins:target-platform-utils</a>, and validate results

<pre>

    # point BASEDIR to where you have these sources checked out
    BASEDIR=$HOME/jbosstools-discovery/jbtcentraltarget; # or, just do this:
    BASEDIR=`pwd`

    # set path to where you have the latest compatible Eclipse bundle stored locally
    ECLIPSEZIP=${HOME}/tmp/Eclipse_Bundles/eclipse-jee-luna-M6-linux-gtk-x86_64.tar.gz
    # set URL for JBT / JBT Target so that all Central deps can be resolved
    UPSTREAM_SITE=http://download.jboss.org/jbosstools/updates/nightly/core/master/

    # Step 1: Merge changes in new target file to actual target file
    pushd multiple && mvn -U org.jboss.tools.tycho-plugins:target-platform-utils:0.19.0-SNAPSHOT:fix-versions -DtargetFile=jbtcentral-multiple.target && rm -f jbtcentral-multiple.target jbtcentral-multiple.target_update_hints.txt && mv -f jbtcentral-multiple.target_fixedVersion.target jbtcentral-multiple.target && popd 

    # Step 2: Resolve the new 'multiple' target platform and verify it is self-contained by building the 'unified' target platform too
    # TODO: if you removed IUs, be sure to do a `mvn clean install`, rather than just a `mvn install`; process will be much longer but will guarantee metadata is correct 
    mvn install -DtargetRepositoryUrl=file://${BASEDIR}/multiple/target/jbtcentral-multiple.target.repo/
    # Step 3: Install the new target platform into a clean Eclipse JEE bundle to verify if everything can be installed
    INSTALLDIR=/tmp/jbtcentraltarget-install-test
    INSTALLSCRIPT=/tmp/installFromTarget.sh
    rm -fr ${INSTALLDIR} && mkdir -p ${INSTALLDIR}
    pushd ${INSTALLDIR}
      echo "Unpack ${ECLIPSEZIP} into ${INSTALLDIR} ..." && tar xzf ${ECLIPSEZIP}
      echo "Fetch install script to ${INSTALLSCRIPT} ..." && wget -q --no-check-certificate -N https://raw.githubusercontent.com/jbosstools/jbosstools-build-ci/master/util/installFromTarget.sh -O ${INSTALLSCRIPT} && chmod +x ${INSTALLSCRIPT} 
      echo "Install..." && ${INSTALLSCRIPT} -ECLIPSE ${INSTALLDIR}/eclipse -INSTALL_PLAN ${UPSTREAM_SITE},file://${BASEDIR}/multiple/target/jbtcentral-multiple.target.repo/ \
      | tee /tmp/log.txt; cat /tmp/log.txt | egrep -i "could not be found|FAILED|Missing|Only one of the following|being installed|Cannot satisfy dependency"
    popd

</pre>

<ol><li value="4"> Check in updated target files & push to the branch.</li></ol>
