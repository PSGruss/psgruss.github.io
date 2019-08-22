---
layout: page
title: The History of Scripting on Windows
---

# In the beginning...

There was DOS. The good ol Disk Operating System. DOS was the backbone of the Windows operating system in those days, and can still be found by launching cmd.exe. Scripting in DOS was done by creating batch files, with the .bat extension. This allowed you to store up a bunch of commands, and execute them. Using things like `GOTO:`, you could add some logic and conditionally run or loop commands.

## VBScripting

Windows 95 introduced the Windows Scripting host, which let you script in different language than DOS. Scripts written in the VBScript and JavaScript languages could now be ran, which expanded the capabilities of scripts that could be ran. These could eventually interact with .Net elements, and really take the possibilities of the command line to the next level. 

## Why do we need PowerShell then?

While VBscript can accomplish a lot, there are some downsides of the "old way" of doing things. A lot of the elements you needed to run scripts were executables on the computer. This meant that there wasn't a very reliable way of making sure your script would run from one machine to the next. If your target machine was a patch behind, or didn't have a certain application installed, then you couldn't use your script. 

In comes PowerShell. PowerShell bridges the gap between Bash on Unix and Command Prompt/DOS on Windows. Unix commands return text, and provide tools like `grep` to parse for the desired data. This works well for Unix because of the OS's architecture, but doesn't work well with Windows' architecture. Powershell commands return Objects, with Methods, Properties, and Types that can be filtered and processed. This allowed for a better interaction between PowerShell and other components of the OS, and turned it into the powerhouse that it is today!
