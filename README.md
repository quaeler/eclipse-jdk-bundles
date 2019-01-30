# eclipse-jdk-bundles
Eclipse features for wrapping Linux, Mac, and Windows OpenJDK binaries

The included maven build structure produces Eclipse bundles and accompanying feature wrapping the OpenJDK binaries for use in RCP applications via p2 touchpoint instructions; the current master tip is configured to wrap OpenJDK 11.0.2


Updating the features version can be performed in this fashion (assuming the current version is 11.0.2.1, and presuming the versioning is done to reflect a feature update wrapping a dot-dot-dot update to 11.0.2)
```
mvn org.eclipse.tycho:tycho-versions-plugin:1.0.0:set-version -DnewVersion=11.0.2.2
```


This work would have taken incredibly longer to arrive without the massive launching point provided by, and of whose works this is largely derivative, [Andreas Buchen.](https://github.com/buchen/bundled-jre)

