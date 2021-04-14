---
title:  "Netbeans V12.x - Platform - Release External Files"
categories: [netbeans platform]
tags: [netbeans platform, release, external, file]
classes: wide
---

If there are additional files, that needs to be installed for a Netbeans Platform Module or Application there are multiple possibilities. You could put it into the installer or somehow add it externally. Some files, like the etc/conf file is configured with the maven nbm plugin. A quite neat method is to deliver those files with a Netbeans Platform Module itself.

## Release external files with your Netbeans Platform Module

The advantage of this method is, that those files are installed if the module is installed. Therefore this works even for additional new modules.

Copy the additional files into a directory into your maven module, e.g. **src/main/release**. You can create a directory structure in this directory as well. This well be created while installing the module. Now configure the **nbm-maven-plugin** to include these files into the module build using **nbmResource**.

```xml
<plugin>
    <groupId>org.apache.netbeans.utilities</groupId>
    <artifactId>nbm-maven-plugin</artifactId>
    <extensions>true</extensions>
    <configuration>
        <nbmResources>
            <nbmResource>
                  <directory>src/main/release</directory>
                  <targetPath>./</targetPath>
                  <includes>
                        <include>**/*.*</include>
                </includes>
            </nbmResource>
        </nbmResources>
    </configuration>
</plugin>
```

These files are part of the nbm but not of the jar file itself and will be independently from the jar file installed into the netbeans platform application.

A normal platform application has a directory structure like this.

```dir
├── bin
├── etc
├── extra
├── ide
├── myappdir
├── java
├── platform
```

With the **targetPath** configuration of the **nbm-maven-plugin** you adressing directories directly under **myappdir**. All included directories from your **nbmResource** are recursivly created under it. So it results in something like:

```dir
├── bin
├── etc
├── extra
├── ide
├── myappdir
│   ├── additionalinstalledfiles
│   │   ├── file1
│   │   ├── file2
│   ├── file3
├── java
├── platform
```
