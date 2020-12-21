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