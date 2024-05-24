# Repackage OpenJFX for OSGi

This repository contains build code to repackage OpenJFX for OSGi. It consumes the published [OpenJFX artifacts from Maven Central](https://mvnrepository.com/artifact/org.openjfx) and puts them unchanged into new artifacts with an additional MANIFEST.MF file.

The MANIFEST.MF file contains the `Java-Module:` header, which is inspected by the [e(fx)clipse Runtime FXClassLoader hook](https://github.com/eclipse-efx/efxclipse-rt/blob/master/modules/core/org.eclipse.fx.osgi/src/main/java/org/eclipse/fx/osgi/fxloader/FXClassLoader.java). This hook puts the embedded OpenJFX jars on the module path, which is required by JavaFX.

It additionally creates Eclipse Features and a p2 update site, to make it easy to consume the build results in an Eclipse RCP based project.

## Build

You can run the build via 

```
mvn clean verify
```

To build a specific OpenJFX revision, specify the `revision` system property, e.g.:

```
mvn clean verify -Drevision=22.0.1
```

To build the p2 update site, you need to activate the `p2` profile:

```
mvn clean verify -Pp2 -Drevision=22.0.1
```

## Note
There is an issue with regards to the linux-aarch64 projects. Not every update on Maven Central contains linux-aarch64 artifacts, and therefore the build breaks because the resources can not be found.

This issue needs to be addressed to make it work even for such updates.
