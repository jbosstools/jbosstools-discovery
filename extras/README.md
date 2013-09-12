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
    BASEDIR=`pwd`
    # Merge changes in new target file to actual target file
    pushd multiple && mvn -U org.jboss.tools.tycho-plugins:target-platform-utils:0.16.0-SNAPSHOT:fix-versions -DtargetFile=locus-multiple.target && rm -f locus-multiple.target locus-multiple.target_update_hints.txt && mv -f locus-multiple.target_fixedVersion.target locus-multiple.target && popd
    # Resolve the new 'multiple' target platform and verify it is self-contained by building it
    mvn -U install -DtargetRepositoryUrl=file://${BASEDIR}/multiple/target/locus-multiple.target.repo/
</pre>

<ol><li value="4"> Check in updated target files & push to the branch.</li></ol>
