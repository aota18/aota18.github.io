---
layout: post
title: "What is Shebang Line?"
date: 2021-06-05 12:11:00 +0900
categories: [Unix]
tags: ["Unix"]
---

```
#!/bin/bash
#!/user/bin/env python3
```

# What is Shebang line?

**Shebang line** is a common pattern for Unix based systems.
It allows a script to be invoked from the command line.

There are two parts to the shebang line.

- _The hash mark and the exclamation sign_ : must be the first two characters on the first line of the file. the exclamation sign is sometimes called as bang. so hash plus bang is where the word shebang comes from. These two letters must be the first two bytes of the file.
- _Path of executable file_ : After the shebang, it is the path to the executable that will run your script along with any other optional arguments.

It is using the Unix env command to find the path to the Python interpreter. Of course, this shebang line doesn't cover all of different installation environments.
