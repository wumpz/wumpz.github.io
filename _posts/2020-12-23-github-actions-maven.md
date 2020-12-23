---
title:  "GitHub Actions and Maven"
categories: [java, github]
tags: [java, maven, github, actions]
classes: wide
---

Since I maintain some OpenSource projects its important to know the projects state. Those projects are using Maven. So a simple local build and test is an easy task. However, the state should be visible right from the projects main page. So at the moment I use **TravisCI** to build my projects and from it I derive some nice build - badges, to show the state and the build result. I am no **TravisCI** expert, I just use it with this simple script:

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

# The simple start 

GitHub provides a very simple way to start. You could follow it here for maven: (https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-java-with-maven). 

Or just use the **Action** menu of your GitHub repository and click **Setup this workflow** for **Java with Maven**. This will generate this workflow script for you:

```yaml
# This workflow will build a Java project with Maven
# For more information see: https://help.github.com/actions/language-and-framework-guides/building-and-testing-java-with-maven

name: Java CI with Maven

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```

> Note: All those configured workflows of your projects are stored within a directory **.github/workflows** and are therefore part of your repository. 

So at first a simple explanation to what this does. So the basic structure of an action or workflow. All this is a **YAML** file.

It starts simply with some naming stuff:

```yaml
name: Java CI with Maven
```

Then we come to the event section, which defines, on which events this action should be started. Here it starts on **push** and **pull** events on your **master** branch.

```yaml
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
```

And last but not least, the jobs itself. What should be done running this action. Here the **steps** will run on a linux (**ubuntu-latest**). So it will checkout the latest sources, setup a JDK 1.8 and runs a maven **-B package --file pom.xml** command.

```yaml
jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```

So thats it.

Under actions you will see the list of runs for your action(s). From here ( **...** button) you can create the needed **build status** badge.

# Building on multiple JDKs 

Since I want to have JDK 11 and above ready projects, this workflow needs to be configured to run the build on multiple JDKs.

To get this kind of **for each** loop, I use the matrix strategy of GitHub Actions. You provide a list of values, that the workflow iterates through or even processes in parallel.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix: 
        java: [8, 11]
    name: Java ${{ matrix.java }} building ... 
    steps:
    - uses: actions/checkout@v2
    - name: Set up Java ${{ matrix.java }} 
      uses: actions/setup-java@v1
      with:
        java-version: ${{ matrix.java }} 
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```

You see this new section **strategy**. There are our Java versions definied: 8 and 11. Within the steps we can now use the actual value using this kind of macro **`${{ matrix.java }}`**.

# Conclusion

So that was easy. Its a bit more complex than **TravisCI** build definition, but I kind of like it. Additionally there are dozens of ready to use workflows available for **deployment**, **publishing**, **building**, **maintaining** your repository. 

* **Greetings**: greets first time contributors for a project
* **Stale**: labels stale issues and pull requests
* ...

For public projects you have 2,000 Actions minutes/month
free. For normal sizes this is sufficient, but you can purchase more.