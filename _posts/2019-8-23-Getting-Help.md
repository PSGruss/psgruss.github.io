
---
layout: page
title: Using Built-In Help Tools
---

# First, some terminology

We use `cmdlets` to interact with PowerShell. All cmdlets are created in a `Verb-Noun` format, followed by `-parameters`. Some parameters are `positional`, and do not require adding the parameter. An example of this:

```powershell
# Show the contents of the current user's profile
Get-Childitem $env:userprofile
Get-Childitem -path $env:userprofile
# Copy the desktop and all it's contents to a temporary location
Copy-Item $env:userprofile\desktop c:\temp -recurse
Copy-Item -path $env:UserProfile\Desktop -destination C:\Temp -recurse
```

Both the `Get-ChildItem` and the `Copy-Item` cmdlets have `aliases`. Aliases are available for cmdlets that are frequently used so you can access them quickly in the console. `Get-ChildItem` can be called using the `dir` or `ls` aliases. `Copy-Item` can be called using `copy`. A list of aliases can be found using the cmdlet `Get-alias`, and new aliases can be created using `Set-alias`. It is recommended to avoid using aliases when creating scripts, but they're fine to use in the console or for quick 1-off scripts that you delete moments after running.

## Using PowerShell to learn PowerShell

Now that a few basics are out of the way, lets look at PowerShell's help system. In *nix systems, you can learn about a command with the `man` command. This is short for Manual, and will open a help file in vi to tell you the syntax of a command and show some successful examples. Powershell has `Get-Help` for this. Here is an example of it's usage:

```powershell
# Use Get-help to get help on get-help
Get-Help get-help
#update get-help to support newer cmdlets
update-help
#use get-help to launch the comparison operators help page
get-help about_comparison_operators -online
```

`Get-Help` is great for learning a new command and troubleshooting errors. the `-detailed` parameter will tell you which parameters are required, positional, accepted from the pipeline, etc. It will also show the item type that the parameter is looking to accept. The `-examples` parameter will show you syntax examples for the command. 

Next up is `Get-Command`. Get-Command will show you available commands that match your query. Here are some examples:

```powershell
# Find commands ending in 'Object'
Get-Command *object
# Find DSC commands in the Az.Automation module
get-command *dsc* -module Az.Automation
# Find all commands with the Set verb in the Az.Automation module
Get-command -verb Set -module Az.Automation
```

This command is great for learning the capabilities of new modules. These two commands are a great starting place for any new PowerShell user.
