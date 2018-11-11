---
title:  "Netbeans - Platform - Localization - Maven"
categories: [netbeans platform]
tags: [netbeans platform]
header:
  image: /assets/images/rocket_launch_2000.jpg
---

# Localization of Modules

If one wants to support multiple languages for Netbeans Modules, then you will probably create some **Bundle.properties** - files to adapt to additional languages.

Foreign modules are localizable using a **branding-project**. Here a list of all bundle files is modifiable. Those values overwrite the originals.

# Localization of Netbeans owns modules

From netbeans websites you can download the complete localization data for the whole **platform**. (**nb71_platform_l10n.zip** you will find by google). This file is to include into your **pom.xml**.

# Workflow to lokalize of all modules of a maven project

1. download the needed localized distribution **Netbeans**.
2. extract your needed localization data using the following script for windows:

    xcopy *_de.jar d:\temp\nb711_de_i10n\ /S /Y
    pause

Now all needed **Bundle** files are copied into your temp folder.

3. the created directory **nb711_de_i10n** should be packed into a ZIP. (ca. 2MB)
4. now you can adapt your branding module
    1. copy **nb711_de_i10n.zip** to **src/resources**. Please make sure it is not under **main** or **test**. With this it will not be catched from mavens packager.
    2. uncompress the ZIP - file in target folder while building

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
					<unzip src="src/resources/nb711_de_l10n.
    zip" dest="target/l10n" overwrite="true"/>
				</tasks>
			</configuration>
		</execution>
	</executions>
</plugin>
```

    3. now let select the needed files and putting it into the jar using your nbm plugin

```xml
<plugin>
	<groupId>org.codehaus.mojo</groupId>
	<artifactId>nbm-maven-plugin</artifactId>
	<configuration>
		<nbmResources>
 <!-- platform data -->
			<nbmResrouce>
				<directory>target/l10n/nb711_de_i10n/platform</directory>
				<includes>
					<include>**/*_de.jar</include>
				</includes>
			</nbmResrouce>
 <!-- ide data -->
			<nbmResrouce>
				<directory>target/l10n/nb711_de_i10n/ide</directory>
				<includes>
					<include>**/*project*_de.jar</include>
					<include>**/*editor*_de.jar</include>
				</includes>
			</nbmResrouce>
		</nbmResources>
	</configuration>
</plugin>
```
