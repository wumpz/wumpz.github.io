---
title:  "Netbeans V23.x - Platform - Custom Notify Descriptor"
categories: [netbeans platform]
tags: [netbeans platform, notify]
classes: wide
---

# Customize Notify Descriptor for Netbeans Platform Applications


Using the notify descriptor this is how you change the button labels in your notification dialogs. 

```java
String selectedValue = (String)DialogDisplayer.getDefault().notify(
  new NotifyDescriptor(
          "this is my message",
          "my dialog title ...", 
          NotifyDescriptor.YES_NO_OPTION, 
          NotifyDescriptor.QUESTION_MESSAGE, 
          new String[] {"all", "some"}, "all"));
```

For more infomation look into javadoc of [NotifyDescriptor](https://bits.netbeans.org/23/javadoc/org-openide-dialogs/org/openide/NotifyDescriptor.html).
