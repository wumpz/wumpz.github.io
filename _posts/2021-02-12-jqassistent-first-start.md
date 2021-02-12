---
title:  "JQAssistent and Maven"
categories: [java, JQAssistent, Maven]
tags: [java, maven, JQAssistent]
classes: wide
---

# First steps: failure for multi module maven project

Following the tutorial [here](https://jqassistant.org/get-started/) for maven projects. This suggests the following: 

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
This works well for a single module maven projects. Since I used it on a multi module project, you will face problems. JQAssistant tries to hold the running Neo4J database open, to access it more performantly. This seems to have some problems in my situation. 

# Success for multi module maven project

> Note: If there are **extensions to the module reactor** ( annotation processors and stuff ), there are problems holding open the Neu4j database.

The solution is to configure jqassistant to close and open the database for each module by configuring **storeLifecycle** to **MODULE**.

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

# Maven profile

I configure all that stuff into a maven profile, to keep it all together. Since there is no magic in this here the profile without any comment and plugins configured.

```xml
<profile>
    <id>process-jqassistant</id>
    <build>
        <plugins>
            <plugin>
                <groupId>com.buschmais.jqassistant</groupId>
                <artifactId>jqassistant-maven-plugin</artifactId>
                <version>1.9.0</version>
                ...
            </plugin>
        </plugins>
    </build>
</profile>
```

# Start the server

After your local Neo4j database is generated you can start a local server with **jqassistant:server** to start a Neo4j server. The Neo4j browser can be started via **http://localhost:7474**.

## first simple Cypher queries

To request **a single class** you match its name within the pattern.
```sql
MATCH (my:Java:Class {name:'MyClass'})
RETURN my
```
or filter it in the **where** clause.

```sql
MATCH (my:Java:Class)
WHERE my.name='MyClass'
RETURN my
```

But be sure what you are doning. The latter could result in a complete node list scan.

Now we want to return its children as well. The **optional match** seems to be something like a left join in SQL.

```sql
MATCH (my:Java:Class {name:'MyClass'})
OPTIONAL MATCH (p:Java)<-[:DEPENDS_ON]-(my)
RETURN my, p
```

## Reseting styling

When you have a bunch of colors assigned to labels, you will find, that you cannot unset a color for a label. Using the following as a query, you can reset all the styling.

```sql
:style reset
```

# Dashboard is a very good start

Using the standard dashboard you are getting some basic values from your fresh generated Neo4j database. By the way you started this using: **http://localhost:7474/jqassistant/dashboard**.

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


# virtual nodes and relations

https://neo4j.com/labs/apoc/4.1/virtual/

Some commands create nodes and relationships but you only want to display this stuff. Thats for you.