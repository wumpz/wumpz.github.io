---
title:  "liquibase and the table / column name case"
categories: [java]
tags: [java, liquibase]
---

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
tbd 

## rewrite names
tbd
