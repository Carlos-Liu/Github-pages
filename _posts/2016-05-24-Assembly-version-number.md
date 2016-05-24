---
layout: post
title:  Assembly Version Number
date:   2016-05-24 13:48:04
author: Carlos Liu
categories: 
---

Each assembly has a version number as part of its identity. As such, two assemblies that differ by version number are considered by the runtime to be completely different assemblies. This version number is physically represented as a four-part string with the following format:

```
<major version>.<minor version>.<build number>.<revision>
```

* Major - set manually
* Minor - set manually
* Build - incremented by 1 each day
* Revision - Changes (how, I don't know) each time you compile


For example, version ```1.5.1254.0``` indicates 1 as the major version, 5 as the minor version, 1254 as the build number, and 0 as the revision number.
