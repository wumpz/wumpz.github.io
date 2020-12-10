---
title:  "Split and RegExp"
categories: [java]
tags: [java, split, regexp, delimiter, lookahead, lookbehind]
classes: wide
---

# Split and RegExp and Delimiters

- how to return delimiters as well
- where to Split
- interludium regexp lookahead / lookbehind (positive, negative)


```java
System.out.println(Arrays.toString(str.split("/")));
System.out.println(Arrays.toString(str.split("(?=/)")));
System.out.println(Arrays.toString(str.split("(?<=/)")));
System.out.println(Arrays.toString(str.split("(?!/)")));
System.out.println(Arrays.toString(str.split("(?<!/)")));
System.out.println(Arrays.toString(str.split("\\b")));
System.out.println(Arrays.toString("hello//world:here".split("\\b")));
System.out.println(Arrays.toString("///a//b/c///".split("/")));


[a, b, c]
[a, /b, /c]
[a/, b/, c]
[a/, b/, c]
[a, /b, /c]
[a, /, b, /, c]
[hello, //, world, :, here]
[, , , a, , b, c]
```
