# eclipse-jdk-bundles

&hellip; for producing Eclipse features for wrapping Linux, Mac, and Windows x64 OpenJDK binaries

The included maven build structure produces Eclipse bundles and accompanying feature wrapping the OpenJDK binaries for use in RCP applications via p2 touchpoint instructions; the current master tip is configured to wrap OpenJDK 13.0.1 JRE taken from [AdoptOpenJDK](https://adoptopenjdk.net/) - though i have left the POMs with commented out elements to easily switch over to using the 'official' OpenJDK JDK (there is, as of this writing, no download for a Java 13 JRE.)

## Building

The build process can either fetch the JRE/JVM from the remote site, or can reference a local (or mounted) filesystem location. If you're experimenting and looking to do the internet a favor, you could download the Linux, macOS, and Windows x64 gtar and zip balls once and place them in a local directory, appropriately [configuring the POM with the directory location.](org.lamport.openjdk/pom.xml#L19) Also please note that the POMs in jdk-bundles and under are presently configured for the 13.0.1 release; variables need be updated when building for a different release.

Once appropriate version has been set (see the following section) the build can be done with the usual package target, with clean thrown in for good measure as:
```
mvn clean package
```

This will produce, in `org.lamport.openjdk.p2repository/target` both a p2 repository in the directory named `repository` and a zip-ed version of the p2 repository named, varying on the version number set, `org.lamport.openjdk.p2repository-13.0.1.1.zip`


## Versioning

Updating the features version can be performed in this fashion (assuming the current version is 13.0.1.3, and presuming the versioning is done to reflect a feature update wrapping a dot-dot-dot update to 13.0.1)
```
mvn org.eclipse.tycho:tycho-versions-plugin:1.0.0:set-version -DnewVersion=13.0.1.4
```

## Credit

This work would have taken incredibly longer to arrive without the massive launching point provided by, and of whose works this is largely derivative, [Andreas Buchen.](https://github.com/buchen/bundled-jre)
