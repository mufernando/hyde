---
layout: post
title: Dealing with processes
comments: true
tags:
  - All
  - Bash
---

We use the shell to run programs and once we hit Enter this program becomes a process. Typically, you'll have several processes running simultaneously in your machine: your web browser, an excel spreadsheet, a text editor and many others. Sometimes we are working with programs that take a long time to run, so it's important to know how to deal with processes.

After typing a command and hitting Enter, the shell prompt becomes "busy" until the process is finished. However, this can kill your work flow when dealing with long-running programs. Instead of running processes in the foreground, it is possible to run them in the background. One way to do this is by adding a _&_ to the end of the command:

```
$ exampleprogram input.txt &
```

Sometimes, though, we forget to add the _&_ to the command. In this case, we can simply press _ctrl+Z_, what will stop the process, and then run **bg** to put it to the background:

```â
$ exampleprogram input.txt
^Z
[1]+  Stopped                 exampleprogram
$ bg
```

After putting processes in the background, we might want to check what is still running. We can do that with **jobs**:

```
$ jobs
```

You can return a process to the foreground with **fg**. The default behavior of **fg** is to put the first process to the foreground, but you can verify with **jobs** the job ID and put to the foreground any process running in the background. Both commands have the same outcome:

```
$ fg
$ fg %1
```

Killing processes is another common task. If you want to kill a process that is currently running in the foreground, just enter _ctrl+C_ and the process will immediately quit. Similarly, you can put a process that is running in the background to the foreground and kill it using the same command.

A more advanced way of killing processes is using the command **kill**. But be aware that this command is powerful, so don't use it unless you know what you're doing. First, let's take a look at which processes are running in the background.

```
$ top
```

```
Processes: 199 total, 2 running, 12 stuck, 185 sleeping, 947 threads                                                     20:03:24
Load Avg: 1.03, 1.12, 1.35  CPU usage: 4.86% user, 5.59% sys, 89.53% idle  SharedLibs: 12M resident, 9264K data, 0B linkedit.
MemRegions: 58566 total, 955M resident, 22M private, 256M shared. PhysMem: 4075M used (1323M wired), 19M unused.
VM: 491G vsize, 1067M framework vsize, 16896346(0) swapins, 18705827(0) swapouts.
Networks: packets: 51704654/67G in, 28727843/6767M out. Disks: 8360105/158G read, 3259094/130G written.

PID    COMMAND      %CPU TIME     #TH  #WQ  #PORT MEM    PURG CMPRS  PGRP  PPID  STATE    BOOSTS          %CPU_ME %CPU_OTHRS UID
13826  top          2.5  00:00.53 1/1  0    19    2608K  0B   0B     13826 386   running  *0[1]           0.00000 0.00000    0
13819- CVMCompiler  0.0  00:00.16 2    1    28    4552K  0B   3260K  13819 1     sleeping *0[1]           0.00000 0.00000    501
13818  Google Chrom 0.0  00:05.09 12   0    115   103M   0B   41M    261   261   sleeping *0[28]          0.00000 0.00000    501
13812  ocspd        0.0  00:00.03 1    0    16    592K   0B   492K   13812 1     sleeping *0[1]           0.00000 0.00000    0
13811  Google Chrom 0.1  00:34.75 14   0    132   11M    0B   394M   261   261   sleeping *0[28]          0.00000 0.00000    501
```

The first column shows us the Process ID (PID). There are many options to this command (check it out with `$ man kill`), but we will use the following:

```
$ kill -9 PID
```

This covers the basics of process management. But when putting process to the background, don't forget to redirect the **stderr** (`$ exampleprogram input.txt > output.txt 2>stderr.txt &`), otherwise you will lose valuable information!
