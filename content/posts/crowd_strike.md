---
title: "Crowdstrike Outage"
date: 2024-07-22T16:22:31-05:00
draft: false
toc: false
images:
tags: 
  - untagged
---


## What exactly happened with #Crowdstrike outage

### Programmer Error
A #CrowdStrike developer made a mistake while writing code in C++.

### Null Pointer Creation
The code created a pointer variable (Obj* obj) that should have pointed to a specific object in memory containing data. However, due to the error, the pointer remained NULL - meaning it wasn't actually pointing to any valid memory location.

### Missing Null Check
The code tried to use this null pointer (obj) to access information within the object it was supposed to represent. Normally, programmers would check if the pointer is null before using it (like if (obj == NULL) { ... }), but this check was missing.

### Attempting to Access "Nothing"
Because the pointer was null, it essentially pointed to "nothing" in memory. When the code tried to access member variables of the object (like obj->a or obj->b), it was attempting to read data from an invalid memory address calculated based on the null pointer value (e.g., 0x0 + 4).  In the attached snapshot (stack dump), you can see that it is trying to access 0x9c which is invalid. Programmer has forgot to check the validity of the objects and it was eventually "NULL + 0x9C = 0x9C". That's an invalid region of memory.

### Memory Access Violation
Since the program was trying to access memory it wasn't supposed to, Windows recognized this as a potential security threat. To protect the system, Windows crashed the program entirely, resulting in the Blue Screen of Death (BSOD) and the outage.  
Essentially, the code tried to read data from nowhere in memory, which triggered a system crash as a safety measure

![Crowd Strike Console](/images/crowd_strike.png)