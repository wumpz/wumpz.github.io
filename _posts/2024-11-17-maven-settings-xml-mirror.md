---
title:  "Apache Maven mirror settings"
categories: [maven, nexus]
tags: [maven, mirror, snapshot]
classes: wide
---

# Apache Maven mirror settings

Using maven all artifacts are searched first in your local repository then at maven central, or which other repository is defined in your **pom.xml**.

This is a valid configuration if you are developing for your own. But if you have a team that uses the same artifacts and sharing new versions of artifacts of your development an additional caching instance is very interesting.

For this tutorial I use [Sonatype Nexus](https://www.sonatype.com/products/sonatype-nexus-repository). There is a free OSS version available that caching stuff very good. 

All artifacts needed to be downloaded once cached in nexus and from this time on all requests are answered from Nexus. So first of all the mirror configuration in your setting.xml forces each external repository search through Sonytype Nexus.

```xml
<mirror>
  <id>my-central-mirror</id>
  <name>my-central-mirror</name>
  <url>http://mynexus-server/repository/maven-public/</url>
  <mirrorOf>*</mirrorOf>
</mirror>
```

Using this wildcard is needed to route all external requests through Sonatype Nexus.

 > [!NOTE]
 > This configuration does not allow SNAPSHOT artifacts to be externally requested. In most of the cases this should be not needed. However the commented part in the example settings file solves this problem as well.

Here the complete configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <mirrors>
        <mirror>
            <id>my-central-mirror</id>
            <name>my-central-mirror</name>
            <url>http://mynexus-server/repository/maven-public/</url>
            <mirrorOf>*</mirrorOf>
        </mirror>
    </mirrors>
	
    <!--  this part is needed to allow external SNAPSHOTs -->
    <profiles>
        <profile>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <repositories>
                <repository>
                    <id>nexus-public</id>
                    <name>Nexus Public Repository</name>
                    <url>http://mynexus-server/repository/maven-public/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
        </profile>
    </profiles>
</settings>
```

> [!NOTE]
> If Nexus is running, you can deploy your generated artifacts into it to share those with your team.