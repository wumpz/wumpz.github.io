

This is a simple document to somehow collect the themes to write about:

* replace JavaScript Engine für Ant - Scripts with Rhino (not GraalVM) https://www.programmersought.com/article/8635310567/

* JSqlParser Gültigkeitsblöcke für verwendete Objektnamen 
  * tatsächliche Tabellen und Spaltennamen
  * verwendete Aliase in welchem Source - Code Bereichen
  * Aufbau eines Gültigkeitsbaums (gültig von bis: Tabellennamen, Aliasnamen, Spaltennamen)

* JDBC to Cypher Transformation possible? 
  * block Auswertung JSqlParser vorher notwendig
  * openCypher ist eine Implementierung
  * JSqlParser -> openCypher

  * Headless Swingtests using CacioCavallo (there is a Java 11 compatible fork at github)
       ```
       <dependency>
            <groupId>com.github.caciocavallosilano</groupId>
            <artifactId>cacio-tta</artifactId>
            <scope>test</scope>
            <version>1.11</version>  <!-- java 11 -->
            <version>1.17</version>  <!-- java 17 -->
        </dependency>
        <dependency>
            <groupId>org.assertj</groupId>
            <artifactId>assertj-swing</artifactId>
            <version>3.17.1</version>
            <scope>test</scope>
        </dependency>
        ```

        ```
        @Category(com.github.caciocavallosilano.cacio.ctc.junit.CacioAssertJRunner.class)
        @RunWith(CacioAssertJRunner.class)
        ```
  * Hyperjaxb3 
      ```
      <plugin>
                <groupId>org.patrodyne.jvnet</groupId>
                <artifactId>hisrc-hyperjaxb-maven-plugin</artifactId>
                <version>0.6.3</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <variant>jpa2</variant>

                    <extension>true</extension>
                    <verbose>false</verbose>
                    <debug>false</debug>
					
                    <includes>
                        <include>gml_3_2_1.xjb</include>
                        <include>jaxb_mapping.xjb</include>
                    </includes>
					
                    <schemaDirectory>src/main/resources/NAS_6.0.1/schema</schemaDirectory>
                    <schemaIncludes>
                        <include>aaa.xsd</include>
                    </schemaIncludes>
                    <persistenceUnitName>jaxb-alkis-6-0-1</persistenceUnitName>
                    <args>
                        <arg>-XautoNameResolution</arg>
                        <arg>-Xinject-code</arg>
                        <arg>-Xnamespace-prefix</arg>
                    </args>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>com.gingko.nas</groupId>
                        <artifactId>alkis.hyperjaxb3.naming</artifactId>
                        <version>${project.version}</version>
                    </dependency>
                    <dependency>
                        <groupId>org.jvnet.jaxb2_commons</groupId>
                        <artifactId>jaxb2-namespace-prefix</artifactId>
                        <version>1.3</version>
                    </dependency>
                </dependencies>
            </plugin>

      ``` 

* streaming group by

* microprofile rest client

* menu with extra buttons layout (refer to older StackOverflow question)

* -Djava.locale.providers=COMPAT,CLDR try to explain this strange jeps changes 

* Nexus 3 configuration for maven
  * Cleanup maven
    * remove cleaned up artifacts
    * clean snapshot multiple versions

* UmlDoclet for JavaDoc
  * easy integration (maven central available)
  * without GraphViz installation (pure Java solution of PlantUML)

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
                        <version>2.0.15</version>
                    </docletArtifact>
                    <additionalOptions>
                        <additionalOption >--create-puml-files</additionalOption>
                        <additionalOption >--uml-custom-directive='!pragma layout smetana'</additionalOption>   <!-- here happens the magic -->
                    <!--<additionalOption>...</additionalOption>-->
                    </additionalOptions>
                    <!-- *END* UML generation -->
                    <useStandardDocletOptions>true</useStandardDocletOptions>
                    <maxmemory>800m</maxmemory>
                    <doclint>none</doclint>				
                </configuration>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

* using maven build cache extension
  * together with sonar
  * with deploy
```xml
<extensions xmlns="http://maven.apache.org/EXTENSIONS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/EXTENSIONS/1.0.0 http://maven.apache.org/xsd/core-extensions-1.0.0.xsd">

    <extension>
        <groupId>org.apache.maven.extensions</groupId>
        <artifactId>maven-build-cache-extension</artifactId>
        <version>1.2.0</version>
    </extension>

</extensions>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<cache xmlns="http://maven.apache.org/BUILD-CACHE-CONFIG/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://maven.apache.org/BUILD-CACHE-CONFIG/1.0.0 https://maven.apache.org/xsd/build-cache-config-1.0.0.xsd">
    <configuration>
        <attachedOutputs>
					<dirNames>
						<dirName>classes</dirName>
						<dirName>test-classes</dirName>
						<dirName glob="jacoco.xml"></dirName>
						<dirName>surefire-reports</dirName>
					</dirNames>
				</attachedOutputs>
				<local>
					<maxBuildsCached>5</maxBuildsCached>
				</local>
				<!-- 
				Since deployment with new versions needs to be done.
				This should only be needed for GM Box build
				--> 
				<projectVersioning calculateProjectVersionChecksum="true" />
    </configuration>
		<executionControl>
        <reconcile>
            <plugins>
							  <!-- 
								this configuration ensures rebuilding if cache was build with tests skipped by command line options 
								--> 
                <plugin artifactId="maven-surefire-plugin" goal="test">
                    <reconciles>
                        <reconcile propertyName="skip" skipValue="true"/>
                        <reconcile propertyName="skipExec" skipValue="true"/>
                        <reconcile propertyName="skipTests" skipValue="true"/>
                        <reconcile propertyName="testFailureIgnore" skipValue="true"/>
                    </reconciles>
                </plugin>
            </plugins>
        </reconcile>
    </executionControl>
</cache>
```
