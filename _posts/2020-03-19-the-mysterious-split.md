---
title:  "the mysterious split"
categories: [java]
tags: [java, split]
---

# Splitting with empty parts

Splitting strings is an everyday job therefore I was quite surprised when I had to do a comma split and wanted to return the emtpy parts as well, that JDK has a solution for this (after years doing it differently). This may not new to you, but it was to me and showed me, that Javas standard library is after this time still good for surprises :).

So given a simple comma list like `1,2,3,4` the split via `"1,2,3,4".split(",")` does a fine job. But if you have something like `1,2,3,4,,` it will skip the empty strings at the end.

Now comes my aha moment. 

`split` supports a second parameter called `limit`. Setting this one to a value below zero it does the job.

`"1,2,3,4,,".split(",")` returns `[1, 2, 3, 4]`.

`"1,2,3,4,,".split(",", -1)` returns `[1, 2, 3, 4, , ]`.
