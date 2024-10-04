---
title:  "Netbeans V12.x - Platform - Custom Notify Descriptor"
categories: [netbeans platform]
tags: [netbeans platform, notify]
classes: wide
---

# Customize Notify Descriptor for Netbeans Platform Applications

```
  String selectedValue = (String)DialogDisplayer.getDefault().notify(
                    new NotifyDescriptor(
                            org.openide.util.NbBundle.getMessage(InstallerUIUtils.class, "CONFIRM_CHOOSE_UPDATEABLE_SOLUTIONS"),
                            "Lösungen ermitteln ...", NotifyDescriptor.YES_NO_OPTION, NotifyDescriptor.QUESTION_MESSAGE, new String[] {"Alle", "Ältere"}, "Alle"));
```
