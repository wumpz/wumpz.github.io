---
title:  "liquibase and the table / column name case"
categories: [java]
tags: [java, liquibase]
---

# Liquibase start

To build up a database from changelogs using Java one could use the following simple code block:

    Liquibase liquibase = new Liquibase(changelogFileName,
                new ClassLoaderResourceAccessor(),
                new DerbyConnection(con));
    liquibase.update("");
    
`con` is a simple JDBC database connection. `ClassLoaderResourceAccessor` loads changelogs via class resource loader. 
Therefore you are able to use changelogs from within a JAR file.

# The Problem

Since liquibase does not change column names in any case.
It is a huge problem if one tries to merge changelogs from different databases with different case 
usages of exactly the same column or table or if you want to pump a changelog from Postgresql to
maybe Oracle. Postgresqls standard nameing is lower case while Oracle uses upper case. Without any
changes you have to escape all your object names:

    select "mycol" from "mytable" where "mycol" like '...'  
    
# The solution (part 1) 

Liquibase comes with the possibility to skip quotation for any SQL statement. Therefore the database 
is responsible to put your object names in the right case. 

Something like

    <insert tableName="mytable">
      <column name="mycol" valueNumeric="1"/>
    </insert>
    
would result in something like 

    insert into mytable (mycol) values (1)
    
which produces the right result. 

But if your names contain some characters not valid for `Locale.US` then it seems to quote this anyway 
and your character case is wrong.

    insert into "mytableäöü" (mycol) values (1)
    
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

This simple implementation replaces the input streams with your own. 

Now the replacing stuff (fun part) starts ...

## rewrite names

First of all. We love **regular expressions** ... :) 

tbd
