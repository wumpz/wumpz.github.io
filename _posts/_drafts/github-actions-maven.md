---
title:  "GitHub Actions and Maven"
categories: [java, github]
tags: [java, maven, github, actions]
classes: wide
---

Since I maintain some OpenSource projects its important to know the projects state. Those projects are using Maven. So a simple local build and test is an easy task. However, the state should be visible right from the projects main page. So at the moment I use **TravisCI** to build my projects and from it I derive some nice build - badges, to show the state and the build result. I am no **TravisCI** export, I just use it with this simple script:

```yaml
language: java
jdk:
  - openjdk8
  - openjdk11

after_success:
  - mvn clean
```

This builds my maven project with Java 8 and Java 11 and after a success it cleans it all up again.

So I have heard of GitHub Actions and wanted to give it a try. Since GitHub is a nice place I would prefer, to keep all stuff on one place. The target is to do the same like with this **TravisCI** script.