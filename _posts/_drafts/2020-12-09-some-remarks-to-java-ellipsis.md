---
title:  "Java Ellipsis"
categories: [java]
tags: [java, ellipsis]
classes: wide
---

```java 
public class TestEllipsis {
    public static void main(String args[]) throws UnsupportedEncodingException {
        testMethod("voll","1","2");
        testMethod("ohne");

        testMethod("null", new String[]{"a", "b"});

        testMethod("null", (String) null);
        testMethod("null", (String[]) null);
	}

    public static void testMethod(String txt, String ... params) {
        System.out.println(txt + " --> ");
        System.out.println(params.length);
        for (String item : params) {
            System.out.println(item);
        }
        System.out.println("done");
    }
}
```
