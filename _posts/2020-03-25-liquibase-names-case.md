---
title:  "liquibase and the table / column name case"
categories: [java]
tags: [java, liquibase, regular expressions, matcher]
classes: wide
---

# Liquibase start

To build up a database from changelogs using Java one could use the following simple code block:

```java
Liquibase liquibase = new Liquibase(changelogFileName,
            new ClassLoaderResourceAccessor(),
            new DerbyConnection(con));
liquibase.update("");
```
    
`con` is a simple JDBC database connection. `ClassLoaderResourceAccessor` loads changelogs via class resource loader. 
Therefore you are able to use changelogs from within a JAR file.

# The Problem

Since liquibase does not change column names in any case.
It is a huge problem if one tries to merge changelogs from different databases with different case 
usages of exactly the same column or table or if you want to pump a changelog from Postgresql to
maybe Oracle. Postgresqls standard nameing is lower case while Oracle uses upper case. Without any
changes you have to escape all your object names:

```sql
select "mycol" from "mytable" where "mycol" like '...'  
```
    
# The solution (part 1) 

Liquibase comes with the possibility to skip quotation for any SQL statement. Therefore the database 
is responsible to put your object names in the right case. 

Something like

```xml
<insert tableName="mytable">
    <column name="mycol" valueNumeric="1"/>
</insert>
```
    
would result in something like 

```sql
insert into mytable (mycol) values (1)
```

which produces the right result. 

But if your names contain some characters not valid for `Locale.US` then it seems to quote this anyway 
and your character case is wrong.

```sql
insert into "mytableäöü" (mycol) values (1)
```
    
# The solution (part 2) - rewrite changelogs 

This solution is a two part thing. 

1. First of all we need to access the InputStream to Liquibase somehow. 
2. Then we need to process the InputStream data to rewrite all names with there upper / lower case pendant.

## access InputStream
The `ResourceAccessor` provides the needed method: `getResourcesAsStream`. Here you could replace the returned 
data stream by your own. Liquibase uses the same class and method to get its own resoures, e.g. schema files.
So you have to check the path for your data. 

With the use of **apache.commons** you could easily read the data streams into a string process it and construct a new 
data stream from that. I know one could improve the memory consumption, but this works well for nearly all *normal* changelogs. 

```java
    @Override
    public Set<InputStream> getResourcesAsStream(String path) throws IOException {
        Set<InputStream> resourcesAsStream = super.getResourcesAsStream(path);
        //since internal schema files are read with this as well, we need to 
        //skip to process those.
        if (!path.startsWith("databases/")) {
            return resourcesAsStream;
        }

        Set<InputStream> corrected = new HashSet<>();
        for (InputStream is : resourcesAsStream) {
            String txt = IOUtils.toString(is, StandardCharsets.UTF_8.name());
            is.close();
            corrected.add(new ByteArrayInputStream(txt.getBytes(StandardCharsets.UTF_8)));
        }
        return corrected;
    }
```

This simple implementation replaces the input streams with your own. 

Now the replacing stuff (fun part) starts ...

## rewrite names

First of all. We love **regular expressions** ... :) 

We have the complete changelog in a string. If you look into a changelog, you will see, that you have to replace 
all expressions like 

    tableName="table"
    name="column"
    
and so on. So how to find this. 

The first try would result in something like:

    (?i)name="([^"]+)"
    
Keeping in mind we want the quoted name to be upper / lower case we want the
regular expression to only return those but we want only those that are preceded by this **name** thing. I am
sure you know about positive lookbehind in regular expressions. So the second one is better:

    (?i)(?<=name=)("[^"]*")

Now we know how to select the right places. But how to replace those with a processed group value. The `replaceAll` methods only
support referencing the group value itself, but only without any processing. 

It appears that the regular expression matcher of java has a solution for this.

```java
Pattern pattern = Pattern.compile("(?i)(?<=name=)(\"[^\"]*\")");
Matcher matcher = pattern.matcher( changelogTxt );

StringBuffer buf = new StringBuffer();  // unfortunately this appending stuff does not support StringBuilder

while ( matcher.find() ) {
    matcher.appendReplacement(buf, matcher.group(1).toUpperCase());
}

matcher.appendTail(buf);
```

So from our original text the interesting parts are replaced while building up a new string in the `StringBuffer`.
