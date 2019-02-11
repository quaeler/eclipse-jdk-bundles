# eclipse-jdk-bundles

&hellip; for producing Eclipse features for wrapping Linux, Mac, and Windows x64 OpenJDK binaries

The included maven build structure produces Eclipse bundles and accompanying feature wrapping the OpenJDK binaries for use in RCP applications via p2 touchpoint instructions; the current master tip is configured to wrap OpenJDK 11.0.2 JRE taken from [AdoptOpenJDK](https://adoptopenjdk.net/) - though i have left the POMs with commented out elements to easily switch over to using the 'official' OpenJDK JDK (there is, as of this writing, no download for a Java 11 JRE.)

## Building

The build process can either fetch the JRE/JVM from the remote site, or can reference a local (or mounted) filesystem location. If you're experimenting and looking to do the internet a favor, you could download the Linux, macOS, and Windows x64 gtar and zip balls once and place them in a local directory, appropriately [configuring the POM with the directory location.](jdk-bundles/pom.xml#L19) Also please note that the POMs in jdk-bundles and under are presently configured for the 11.0.2 release; variables need be updated when building for a different release.

Once appropriate version has been set (see the following section) the build can be done with the usual package target, with clean thrown in for good measure as:
```
mvn clean package
```

This will produce, in `update-site/target` both a p2 repository in the directory named `repository` and a zip-ed version of the p2 repository named, varying on the version number set, `st.theori.openjdk.updatesite-11.0.2.9.zip`


## Versioning

Updating the features version can be performed in this fashion (assuming the current version is 11.0.2.1, and presuming the versioning is done to reflect a feature update wrapping a dot-dot-dot update to 11.0.2)
```
mvn org.eclipse.tycho:tycho-versions-plugin:1.0.0:set-version -DnewVersion=11.0.2.2
```


## Problems involving RCPTT, macOS, and Eclipse

Due to the [code check here](https://github.com/xored/rcptt/blob/a37c7c109ee5659b909797b32ed8e252d9f9a387/runner/org.eclipse.rcptt.runner/src/org/eclipse/rcptt/runner/util/AUTsManager.java#L179) which looks to make sure, for reasons unclear, that the parent directory of the JVM executable is named `bin` RCPTT concludes that the macOS feature is invalid (as the parent directory is `lib`) issuing a logging message of the form `[INFO] Unknown file system layout of Java VM from ini file.` This is due to the requirement that the Eclipse `-vm` reference point, on macOS, to a dynamic library and not the `java` executable itself - why this is so: unknown.

Chasing the rabbit down the hole, we could get around this by soft linking the dynamic library to the `bin` directory; if we copy it, the VM will not initialize properly. We can achieve this soft-linking through a touchpoint instruction.

Unfortunately, the touchpoint instruction for linking does not observe the touchpoint global variable `@artifact` which the Eclipse Target Definition resolver understands, it only observes the variable `${installFolder}` while the Eclipse resolver cannot handle correctly.


## Credit

This work would have taken incredibly longer to arrive without the massive launching point provided by, and of whose works this is largely derivative, [Andreas Buchen.](https://github.com/buchen/bundled-jre)

