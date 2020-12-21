---
title:  "JQAssistent and Maven"
categories: [java, JQAssistent, Maven]
tags: [java, maven, JQAssistent]
classes: wide
---

* first start did not work

```xml
<plugin>
                <groupId>com.buschmais.jqassistant</groupId>
                <artifactId>jqassistant-maven-plugin</artifactId>
                <version>1.9.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>scan</goal>
                            <goal>analyze</goal>
                        </goals>
                        <configuration>
                            <warnOnSeverity>MINOR</warnOnSeverity>
                            <failOnSeverity>MAJOR</failOnSeverity>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

* storeLifecycle to MODULE
  * there are some extensions to the module reactor (annotation processors and stuff)
  * this one seems to confuse the access to neo4j database


```xml
<plugin>
                <groupId>com.buschmais.jqassistant</groupId>
                <artifactId>jqassistant-maven-plugin</artifactId>
                <version>1.9.0</version>
                <configuration>
                    <storeLifecycle>MODULE</storeLifecycle>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>scan</goal>
                            <goal>analyze</goal>
                        </goals>
                        <configuration>
                            <warnOnSeverity>MINOR</warnOnSeverity>
                            <failOnSeverity>MAJOR</failOnSeverity>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

* profile
* requests on codebase

* request a class

```sql
MATCH (my:Java:Class {name:'SolutionInformation'})
RETURN my
```

```sql
MATCH (my:Java:Class)
WHERE my.name='SolutionInformation'
RETURN my
```


* class and its children

```sql
MATCH (p:Java)<-[:DEPENDS_ON]-(my:Java:Class)
WHERE my.name='SolutionInformation'
RETURN p
```

does not print the class itself

```sql
MATCH (my:Java:Class)
WHERE my.name='SolutionInformation'
OPTIONAL MATCH (p:Java)<-[:DEPENDS_ON]-(my)
RETURN my, p
```

so optional match seems to be something like a left join

```sql
MATCH (my:Java:Class {name:'SolutionInformation'})
OPTIONAL MATCH (p:Java)<-[:DEPENDS_ON]-(my)
RETURN my, p
```


reseting the color of my assignments

```sql
:style reset
```

virtual nodes and relations

https://neo4j.com/labs/apoc/4.1/virtual/

Some commands create nodes and relationships but you only want to display this stuff. Thats for you.

dashboard is a very good start

```xml
<plugin>
                <groupId>com.buschmais.jqassistant</groupId>
                <artifactId>jqassistant-maven-plugin</artifactId>
                <version>1.9.0</version>
                <configuration>
                    <storeLifecycle>MODULE</storeLifecycle>
                    <scanIncludes>
                        <scanInclude>
                            <path>${project.basedir}/.git</path>
                        </scanInclude>
                    </scanIncludes>
                    <groups>
                        <group>jqassistant-dashboard:Default</group>
                        <!-- Insert your own group references here -->
                    </groups>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>scan</goal>
                            <goal>analyze</goal>
                        </goals>
                        <configuration>
                            <warnOnSeverity>MINOR</warnOnSeverity>
                            <failOnSeverity>MAJOR</failOnSeverity>
                        </configuration>
                    </execution>
                </executions>
                <dependencies>
                    <dependency>
                        <groupId>de.kontext-e.jqassistant.plugin</groupId>
                        <artifactId>jqassistant.plugin.git</artifactId>
                        <version>1.8.0</version>
                    </dependency>
                    <dependency>
                        <groupId>org.jqassistant.contrib.plugin</groupId>
                        <artifactId>jqassistant-dashboard-plugin</artifactId>
                        <version>1.9.0</version>
                    </dependency>
                </dependencies>
            </plugin>
```
