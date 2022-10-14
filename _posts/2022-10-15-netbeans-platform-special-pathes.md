---
title:  "Netbeans V14.x - Platform - Special Pathes"
categories: [netbeans platform]
tags: [netbeans platform, pathes]
classes: wide
---

## How to find Netbeans Platform special pathes

This is a small hint of mine. For those special pathes Netbeans comes with the class `Places`. Here you get the user dir, the cache dir and more.

Here two examples: 

* User directory: `Places.getUserDirectory()`
* Cache directory: `Places.getCacheDirectory()`
     

This artifact has to be installed.

```xml
<dependency>
   <groupId>org.netbeans.api</groupId>
   <artifactId>org-openide-modules</artifactId>
   <version>RELEASE140</version>
</dependency>
```
