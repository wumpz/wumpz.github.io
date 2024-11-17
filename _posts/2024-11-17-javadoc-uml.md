---
title:  "UML diagrams in JavaDocs using Maven"
categories: [maven]
tags: [java, maven, javadoc]
classes: wide
---

## How to include UML diagrams in your JavaDocs

This simple tutorial uses [UMLDoclet](https://github.com/talsma-ict/umldoclet) to produce some nice looking class diagrams within your javadocs. For example images just visit the UMLDoclet.

Since this UMLDoclet is available at maven central, there should be no problem integrating this into your build process. 
  
UMLDoclet uses [PlantUML](https://plantuml.com/) under the hood to generate those images. 

PlantUML has some time now a layouter called smetana, which gets rid of a local graphviz installation. For UMLDoclet previous version 2.2 you need to add this into your PlantUML files to use this. 

```
!pragma layout smetana
```

or within your UMLDoclet configuration

```xml
<additionalOption >--uml-custom-directive='!pragma layout smetana'</additionalOption>
```

Without further explanations here a complete maven javadoc plugin configuration to make use of UMLDoclet.  The interesting part is within the **UML generation** section.

```xml
<plugin>
   <groupId>org.apache.maven.plugins</groupId>
   <artifactId>maven-javadoc-plugin</artifactId>
   <version>3.3.0</version>
   <configuration>
      <!-- *START* UML generation -->
      <doclet>nl.talsmasoftware.umldoclet.UMLDoclet</doclet>
      <docletArtifact>
         <groupId>nl.talsmasoftware</groupId>
         <artifactId>umldoclet</artifactId>
         <version>2.2.1</version>
      </docletArtifact>
      <additionalOptions>
         <additionalOption>--create-puml-files</additionalOption>
         <additionalOption >--uml-custom-directive='!pragma layout smetana'</additionalOption> <!-- not needed anymore -->
      </additionalOptions>
      <!-- *END* UML generation -->
   </configuration>
</plugin>
```

> [!WARNING]
> Versions previous of 2.2.1 of UMLDoclet have a problem if javadoc plugin uses the configuration `<verbose>true</verbose>`. Then a problematic exception is thrown.


Using `--create-puml` as UMLDoclet configuration it creates PlantUML files for further use, maybe write some additional source code documentation. 