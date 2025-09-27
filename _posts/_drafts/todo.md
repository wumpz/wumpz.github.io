

This is a simple document to somehow collect the themes to write about:

* git worktree (support in ides)

* SnakeYaml ouput without tags, Json and stuff

* regexp backtracking (performance)

* replace JavaScript Engine für Ant - Scripts with Rhino (not GraalVM) https://www.programmersought.com/article/8635310567/

* JSqlParser Gültigkeitsblöcke für verwendete Objektnamen 
  * tatsächliche Tabellen und Spaltennamen
  * verwendete Aliase in welchem Source - Code Bereichen
  * Aufbau eines Gültigkeitsbaums (gültig von bis: Tabellennamen, Aliasnamen, Spaltennamen)

* JDBC to Cypher Transformation possible? 
  * block Auswertung JSqlParser vorher notwendig
  * openCypher ist eine Implementierung
  * JSqlParser -> openCypher

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

