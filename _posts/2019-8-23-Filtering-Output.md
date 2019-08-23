---
layout: post
title: Filtering Output
---
# Finding commands to filter with

In my last post, I talked about commands like `Get-Help` and `Get-Command` to find commands to use, and what parameter those commands require. Now, we're going to use them to find commands that we can use to filter our output. In all of my examples, I am either going to use `Get-Process` or `Get-ChildItem` to start, and filter the output. While both of these commands have ways of filtering built in, this will be a good way to show some of the other cmdlets in PowerShell

```powershell
# Find cmdlets we can use to filter outpu
Get-Command *Object
```
The obove will return the following cmdlets:

### Compare-Object

`Compare-Object` is used to compare object. Say you want to know what processes an application spawns after it's launched, you can first store the results of `Get-Process` into a variable, launch the application, then use `Compare-Object` on `Get-Process` again to see what is different. You won't find an exact match here, because of other background processes doing their normal thing, but it'll at least give you an idea.

```powershell
# Store current processes
$Proc = get-process
# launch application through whatever means you'd like
Start-process <path> -argumentlist "<arguments>"
# Compare the live processes with the stored process list
Compare-object $Proc (get-process)
```

Compare-Object has positional parameters for `-ReferenceObject` and `-DifferenceObject`. The order that you put them in doesn't REALLY matter, as long as you know which one is which. The output will point either `<=` or `=>` for which side is different. You can also add `-includeEqual` and `-excludeDifferent` to modify what is returned, based on what you want out of it.

### ForEach-Object

`ForEach-Object` is for looping through objects line-by-line. When a cmdlet, like `get-process`, returns it's output, each line can be considered an independant object, and `ForEach-Object` can loop through each object to perform an action against it. This is a more advanced cmdlet that will be covered in another post.

### Group-Object

`Group-Object` is used to group objects with the same property value. Say someone's computer's running slow, but all they have open is Chrome. As I'm sure we all know, Chrome launches multiple processes to function properly. You want to check how many processes are running to make sure everything's in check. You'd run something like:

```powershell
get-process | group-object Name
```

This will group all output on the Name property, and tell you how many instances are found.

### Measure-Object

`Measure-Object` measures output. `Measure-object` is a really intricate cmdlet that can provide a lot of useful information in a lot of scenarios. Say you want a sum of a large list of values. Say you want the count of objects that are returned. Say you want to quickly know how many characters are being presented. You can use it to find it the largest, smallest, or average value in a list. All in all, there's a lot that this cmdlet can do to help quickly find measurements of output

```powershell
#Find how many files are in a directory
get-childitem c:\windows | measure-object
# Find the size of all of the files in a directory
get-childitem "c:\Windows" -recurse | measure-object -Property Length -sum
### NOTE! I do not recommend doing this on the c:\windows folder. this is just an example.###
#Find the largest file in a directory
get-childitem "C:\Windows" -recurse | measure-object -property length -max
#Find how many characters are in a log file
get-content C:\Logs\File.log | measure-object -character
```

As you can see, there is a lot that this cmdlet can do, and I feel like I've scratched the surface of how it can help you. I highly recommend looking at the help for this command.

### New-Object

`New-Object` is a more advanced cmdlet. This cmdlet allows you to create new objects from either predefined classes or .net resources. I'll go into more details on this one in a later blog post.

### Select-Object

If you walk away from this post knowing anything, it should be `Select-Object`.
