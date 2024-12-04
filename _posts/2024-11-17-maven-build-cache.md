---
title:  "Apache Maven build cache"
categories: [maven]
tags: [java, maven, cache]
classes: wide
---

This tutorial will give some informations on [build cache](https://maven.apache.org/extensions/maven-build-cache-extension/) for Apache Maven. 

To activate the build cache you need some Maven version 3.9.x and above. Then in your project there must be a **.mvn** directory and in there then **extension.xml** file to load the cache extension. Those first steps are very good documented under [Getting Started](https://maven.apache.org/extensions/maven-build-cache-extension/getting-started.html).

**extension.xml**

```xml
<extensions xmlns="http://maven.apache.org/EXTENSIONS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/EXTENSIONS/1.0.0 http://maven.apache.org/xsd/core-extensions-1.0.0.xsd">

    <extension>
        <groupId>org.apache.maven.extensions</groupId>
        <artifactId>maven-build-cache-extension</artifactId>
        <version>1.2.0</version>
    </extension>

</extensions>
```

Now the cache is activated and will work for your builds. However a bit of finetuning is often needed. 

One very usefull maven build parameter is `-Dmaven.build.cache.skipCache=true` which skips reading from the cache but writing to it after a complete build. Sometimes there are unwanted situations cached, which makes problems afterwards. 

When the cache plugin prevents some plugins from running, it tries to rebuild some parts of the target directory. There you need often more than the standard configuration which only rebuilds the generated jar files. 

The cache configuration is stored withtin the file **maven-build-cache-config.xml** which goes in **.mvn** as well. 

Here is my configuration.

**maven-build-cache-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<cache xmlns="http://maven.apache.org/BUILD-CACHE-CONFIG/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://maven.apache.org/BUILD-CACHE-CONFIG/1.0.0 https://maven.apache.org/xsd/build-cache-config-1.0.0.xsd">
    <configuration>
        <attachedOutputs>
            <dirNames>
               <dirName>classes</dirName>
               <dirName>test-classes</dirName>
               <dirName glob="jacoco.xml"></dirName>
               <dirName>surefire-reports</dirName>
            </dirNames>
         </attachedOutputs>
         <local>
            <maxBuildsCached>5</maxBuildsCached>
         </local>
         <projectVersioning calculateProjectVersionChecksum="true" />
    </configuration>
		<executionControl>
        <reconcile>
            <plugins>
               <!-- 
               this configuration ensures rebuilding if cache was build with tests skipped by command line options 
               --> 
                <plugin artifactId="maven-surefire-plugin" goal="test">
                    <reconciles>
                        <reconcile propertyName="skip" skipValue="true"/>
                        <reconcile propertyName="skipExec" skipValue="true"/>
                        <reconcile propertyName="skipTests" skipValue="true"/>
                        <reconcile propertyName="testFailureIgnore" skipValue="true"/>
                    </reconciles>
                </plugin>
            </plugins>
        </reconcile>
    </executionControl>
</cache>
```

To this some remarks.

The **projectVersioning** setting activates the cache to take module versions into account. Since we often build hotfix branches, maven would consider the actual version identical to the previous version. So no build is done and all dependency configurations to those modules reference the wrong version. 

Within the `<attachedOutputs>` section you specify all parts you want to be restored from cache.  Most of my settings are used to make SonarQube run successfully on a from cache restored target directory. Additionally I have the feeling, that missing **classes** and **test-classes** directories somehow influence running IDEs. 

Since we are using local builds, `<maxBuildsCached>` set to 5 seems reasonable.