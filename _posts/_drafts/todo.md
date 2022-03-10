

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

* Java 11 Upgrade
  * JAXB
        ```<dependency>
            <groupId>javax.xml.bind</groupId>
            <artifactId>jaxb-api</artifactId>
            <version>2.4.0-b180830.0359</version>
        </dependency>
        <dependency>
            <groupId>org.glassfish.jaxb</groupId>
            <artifactId>jaxb-runtime</artifactId>
            <version>2.4.0-b180830.0438</version>
            <scope>test</scope>
        </dependency>
          ```
  * Headless Swingtests using CacioCavallo (there is a Java 11 compatible fork at github)
       ```
       <dependency>
            <groupId>com.github.caciocavallosilano</groupId>
            <artifactId>cacio-tta</artifactId>
            <scope>test</scope>
            <version>1.11</version>
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

      ```  * JSqlParser -> openCypher

* streaming group by

* microprofile rest client

* menu with extra buttons layout (refer to older StackOverflow question)

* -Djava.locale.providers=COMPAT,CLDR try to explain this strange jeps changes 

* Nexus 3 configuration for maven
  * Cleanup maven
    * remove cleaned up artifacts
    * clean snapshot multiple versions
