---
title:  "Headless Swing Tests in Java"
categories: [java tests gui]
tags: [java tests gui headless]
classes: wide
---

# Headless Swing Tests in Java

Automated tests of GUIs is allways a problem. Keep in mind, that all tests need to run in a headless environment - your build server runs on a headless machine, right? For browser based applications there are tools. For Swing based GUIs there were strange suggestions building virtual machines that are controlled by the build process. 

Here comes [CacioCavallo](https://github.com/CaciocavalloSilano/caciocavallo) into play. It simulates a graphics display even in a headless environment. Now tests using e.g. **assertj-swing** do work. 

Using maven you need to include this to start

 ```
<dependency>
    <groupId>com.github.caciocavallosilano</groupId>
    <artifactId>cacio-tta</artifactId>
    <scope>test</scope>
    <version>1.11</version>  <!-- java 11 -->
    <version>1.17</version>  <!-- java 17 -->
    <version>1.18</version>  <!-- java 21 -->
</dependency>
<dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-swing</artifactId>
    <version>3.17.1</version>
    <scope>test</scope>
</dependency>
```

Be sure to configure your `surefire` plugin the right way:

```
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>2.12</version>
  <configuration>
    <systemPropertyVariables>
      <java.awt.headless>false</java.awt.headless>
      <awt.toolkit>com.github.caciocavallosilano.cacio.ctc.CTCToolkit</awt.toolkit>
      <java.awt.graphicsenv>com.github.caciocavallosilano.cacio.ctc.CTCGraphicsEnvironment</java.awt.graphicsenv>
    </systemPropertyVariables>
    <argLine>
      --add-exports=java.desktop/java.awt=ALL-UNNAMED
      --add-exports=java.desktop/java.awt.peer=ALL-UNNAMED
      --add-exports=java.desktop/sun.awt.image=ALL-UNNAMED
      --add-exports=java.desktop/sun.java2d=ALL-UNNAMED
      --add-exports=java.desktop/java.awt.dnd.peer=ALL-UNNAMED
      --add-exports=java.desktop/sun.awt=ALL-UNNAMED
      --add-exports=java.desktop/sun.awt.event=ALL-UNNAMED
      --add-exports=java.desktop/sun.awt.datatransfer=ALL-UNNAMED
      --add-exports=java.base/sun.security.action=ALL-UNNAMED
      --add-opens=java.base/java.util=ALL-UNNAMED
      --add-opens=java.desktop/java.awt=ALL-UNNAMED
      --add-opens=java.desktop/sun.java2d=ALL-UNNAMED
      --add-opens=java.base/java.lang.reflect=ALL-UNNAMED
    </argLine>
  </configuration>
</plugin>
```

Using JUnit you need to annotate your class with 

```
@RunWith(CacioAssertJRunner.class)
```
