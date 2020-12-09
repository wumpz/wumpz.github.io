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

```
voll --> 
2
1
2
done
ohne --> 
0
done
null --> 
2
a
b
done
null --> 
1
null
done
null --> 
Exception in thread "main" java.lang.NullPointerException
	at org.tw.ellipsis.TestEllipsis.testMethod(TestEllipsis.java:25)
	at org.tw.ellipsis.TestEllipsis.main(TestEllipsis.java:20)
```
