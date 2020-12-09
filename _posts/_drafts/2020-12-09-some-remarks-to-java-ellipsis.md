---
title:  "Java VarArgs"
categories: [java]
tags: [java, ellipsis, varargs]
classes: wide
---

# Java VarArgs

To support an arbitrary number of arguments of one type for a method you have basically only one option, you have to create an array.

```java
void multiParamArray(String[] args) {}


String[] params = new String[2];
params[0]="param1";
params[1]="param2";

multiParamArray(params);
```

So to use this method you have to build an array that is then used as the **args** parameter. Using this newer syntax, which was introduced in Java 5, hides this complete array creation.

```java
void multiParamVarArg(String ... args);


multiParamVarArg("param1", "param2");
```

Those three dots indicate, that the caller could put an arbitrary number of arguments of the same type (*String*) to this method.

Inernally an array is here used as well. Within your method you process this parameter like a normal array.

# Processing a VarArg parameter

Within your method you have access to normal array methods, like *length* and normal value access via index.

```java
void multiParamVarArg(String ... args) {
  for (String item : args) {
      System.out.println(item);
  }
}
```

So lets discuss multiple different calls to this code. First of all if you don't set this **args** parameter with an array, you have no possibility of getting an **null** array. If you provide no argument, then the **length** of **args** simply will be 0.

```java
multiParamVarArg();       // args.length == 0
multiParamVarArg(null);   // args.length == 1; args[0]==null;
multiParamVarArg("param1", "param2")  ;   // args.length == 2;
```

You still have the possibility to set this parameter using an array.

```java
multiParamVarArg(new String[]{"param1", "param2"});   // args.length == 2;

//this one is a problem
multiParamVarArg((String[]) null)  ;   // args == null;
```
