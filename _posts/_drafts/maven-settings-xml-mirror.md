---
title:  "maven mirror settings and SNAPSHOT artifacts"
categories: [maven]
tags: [maven, mirror, snapshot]
classes: wide
---

what, why, when

* nexus
* all repository requests should be cached through nexus
* mirror
  * snapshots not found why


# maven mirror settings and SNAPSHOT artifacts


<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <!-- first -->
    <mirrors>
        <mirror>
            <id>my-central-mirror</id>
            <name>my-central-mirror</name>
            <url>http://mynexus-server/repository/maven-public/</url>
            <mirrorOf>*</mirrorOf>
        </mirror>
    </mirrors>
	
    <!-- that was missing -->
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

