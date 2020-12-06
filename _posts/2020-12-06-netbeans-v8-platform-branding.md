---
title:  "Netbeans V8.x - Platform - Localization - Maven"
categories: [netbeans platform]
tags: [netbeans platform]
header:
  image: /assets/images/rocket_launch_2000.jpg
classes: wide
---

This is a followup to my previous post about localization of Netbeans. But this one uses the localization files of Netbeans V8.x. Thanx Stefan for providing these informations. So the post text is merely the same but the scripts and file names are different.

> **_TIP:_** This described corrected workflow could be used with higher versions of Netbeans with slight adjustments of the scripts.

# Localization of Modules

If one wants to support multiple languages for Netbeans Modules, then you will probably create some **Bundle.properties** - files to adapt to additional languages.

Foreign modules are localizable using a **branding-project**. Here a list of all bundle files is modifiable. Those values overwrite the originals.

# Localization of Netbeans owns modules

To get **nb82_de_i10n.zip** file that contains all localization data, you have to extract this of your own using a complete install if Netbeans. From this installation you can extract all needed files.

Here I have only a Windows batch script. So if you provide a Linux script I will be happy to add that.

As you see this script only extracts the German localization files. You could adapt that to your language.

```bash
####################################################################################
REM Unpacked localized netbeans build (zip distribution), e.g. netbeans-8.2-201610071157-javase.zip
REM Copy this batch to netbeans-8.2-201610071157-javase\netbeans\ide
SET DEST=..\..\tmp\nb82_de_i10n\platform
xcopy *_de.jar %DEST%\ /f /S /Y
pause
####################################################################################
```

Now you can build **nb82_de_i10n.zip** from this extracted files in **./tmp/nb82_de_i10**.

# Workflow to localize of all modules of a maven project

1. download the needed localized distribution **Netbeans**.
2. extract your needed localization data using the upper workflow to generate the **zip** file.
3. now you can adapt your branding module
  - copy ***nb82_de_i10n.zip** to **src/resources**. Please make sure it is not under **main** or **test**. With this it will not be catched from mavens packager.
  - uncompress the ZIP - file in target folder while building

  ```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-antrun-plugin</artifactId>
    <executions>
        <execution>
              <phase>process-resources</phase>
              <goals>
                  <goal>run</goal>
              </goals>
              <configuration>
                  <tasks>
                      <unzip src="src/resources/nb82_de_i10n.zip"
              dest="target/l10n" overwrite="true"/>
                  </tasks>
              </configuration>
          </execution>
      </executions>
  </plugin>
  ```
  - now let us select the needed files and putting it into the jar using your nbm plugin

  ```xml
  <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>nbm-maven-plugin</artifactId>
      <configuration>
          <nbmResources>
   <!-- platform data -->
              <nbmResrouce>
                  <directory>target/l10n/nb82_de_i10n/platform</directory>
                  <includes>
                      <include>**/*_de.jar</include>
                  </includes>
              </nbmResrouce>
   <!-- ide data -->
              <nbmResrouce>
                  <directory>target/l10n/nb82_de_i10n/ide</directory>
                  <includes>
                      <include>**/*project*_de.jar</include>
                      <include>**/*editor*_de.jar</include>
                  </includes>
              </nbmResrouce>
          </nbmResources>
      </configuration>
  </plugin>
  ```
