---
title:  "Netbeans V29.x - Standard logger"
categories: [netbeans java]
tags: [netbeans java, logger]
classes: wide
---

# Customize Generated Logger for Java applications

Since Netbeans 26.x within Java code generated logger using the `System.Logger. 

```java
private static final System.Logger logger = System.getLogger(MyClass.class.getName());
```

To reset to the old behaviour there si only a weird way within the editors hints. 

Goto options to **Editor/Hints**. Search here for **Surround with try-catch** (in **Java / Error Fixes**). Now isn't that a strange position to hide this settings. 

Here you can uncheck the System.Logger setting. No a **normal** logger is generated again.

```java
private static final Logger logger = Logger.getLogger(MyClass.class.getName());
```

