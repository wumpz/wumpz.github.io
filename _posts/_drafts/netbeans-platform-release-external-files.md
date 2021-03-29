---
title:  "Netbeans V12.x - Platform - Release External Files"
categories: [netbeans platform]
tags: [netbeans platform, release, external, file]
classes: wide
---

# Release external files with your Netbeans Platform Module 


* copy release files in src/main/release with subdirectories


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


these files are part of the nbm but not of the jar

if you include this plugin into a netbeans platform application those external files are extreacted