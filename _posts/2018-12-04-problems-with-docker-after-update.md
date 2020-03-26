---
title:  "Docker Update - Problems"
categories: [docker, systemd]
tags: [docker, systemd]
#header:
#  image: /assets/images/rocket_launch_2000.jpg
classes: wide
---

# Docker

After updating my developer engine the docker service fails to start. The main error message was, that there are no sockets available and due to this the listeners couldn't be loaded.

My docker runs on an Ubuntu 18.04. server.

The problem was, that my configuration contained the old school socket definition via

```
-H fd://
```

To solve my docker problem it has to be replaced by

```
-H unix://
```

But don't forget to reload the service daemon after that, like I did ;).
