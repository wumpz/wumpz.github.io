---
title:  "Split and RegExp"
categories: [java]
tags: [java, split, regexp, delimiter, lookahead, lookbehind]
classes: wide
---

# Split and RegExp and Delimiters

Use Java's **split** method is easy. You split a string at the delimiters.

```java
"a/b/c".split("/")
```

does what you expect it to do, an array containing **[a, b, c]**.

But what if you want to return the delimiters as well. Now we have to use some more regular expression skills.

## return delimiters separately

For this staring string we could use **\b** as the regular expression. This matches word boundaries. So you are able to split our original string between the characters and the **/**.

```java
"a/b/c".split("\\b")
```

returns **[a, /, b, /, c]**.

> Note: Using regular expressions, you are able to select not only indexes of your original string but between indexes as well. In the above example the word boundary is between **a** and **/** or **/** and **b**.

## Interludium: lookahead and lookbehind of regular expressions

Using **lookahead** or **lookbehind** you requesting the context of your match. But the result will not be part of the match itself. We call those **non-capturing**. Both types are available in **positive** or **negative** form, which means looking for something that is there or is not there. Look into the following table to get a grip on that.

name | expression | explanation
-----|------------|-------------
positive lookahead | (?=X) | This expression is useful, if you want to match something which is **followed by** something else, in this case a **X**. So to say you are finding **all things before** a **X**.
negative lookahead | (?!X) | This one finds all things that are **not followed by** a **X**.
positive lookbehind | (?<=X) | This one finds all things that **are following** a **X**.
negative lookbehind | (?<!X) | This one finds all things that not **are following** a **X**.

## split before a **/**

Using our lookahead / lookbehind knowledge we are able to build that kind of regular expression. So we want to split before a **/**, that means at a position, that is followed by a **/**. Therefore we use a lookahead.

```java
"a/b/c".split("(?=/)")
```

returns **[a, /b, /c]**.

## split after a **/**

Now we want to split at positions that follow a **/**. Remember, that this position is somehow between the charaters indexes. Here we need a **lookbehind**.

```java
"a/b/c".split("(?<=/)")
```

returns **[a/, b/, c]**.


# Conclusion

Using special regular expression, we are able to return delimiters of your strings as well in various forms. We love regular expressions ;).
