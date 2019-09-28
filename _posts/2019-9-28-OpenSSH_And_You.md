---
layout: page
title: Using Built-In Help Tools
difficulty: Advanced
---

# OpenSSH On Windows

## What is SSH?

SSH stands for Secure Shell. It is both a protocol and an application that's used to remotely manage devices. Historically, it's been used for accessing \*nix systems, as they have ssh utilities built in. Windows has never natively used ssh, opting instead for Windows Remote Management (winrm). 

SSH can use standard username/password authentication to establish the connection between machines, but typically uses shared key authentication. A shared key authentication system uses a set of keys to establish the secure connection. The server that you're connecting to will hold the Public key, and the client that's connecting will hold the private key. As long as your account has been granted access to connect, and your keys match up, then you can connect!

Microsoft started including OpenSSH Server utilities with Windows 10 and Server 2019 1809. 

## Installing OpenSSH Server

There are a few ways you can install OpenSSH Server. Please note that OpenSSH Server can be installed on Windows machines prior to the 1809 update. In my testing, OpenSSH can be installed as far back as Windows Server 2016 1607.

**1. Add-WindowsCapability or dism**

   This method is for newer Windows machines that have the OpenSSH.Server capability.

   ```powershell
   Get-WindowsCapability -name "OpenSSH.Server*" -online | add-windowscapability -online
   ```
   After this command completes, you will have the sshd service available, but stopped.

**2. Install OpenSSH Server from Github**

   This installation method downloads the OpenSSH files from [Microsoft's OpenSSH Github page](https://github.com/PowerShell/Win32-OpenSSH). Because these files come from an outside source, this method can be used on machines that do not come with the OpenSSH.Server capability. Microsoft has a good set of [instructions on how to install OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH), if you'd prefer to follow those. 
   
   ```powershell
   # First, we need to get the URL of the latest release
   
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   $url = 'https://github.com/PowerShell/Win32-OpenSSH/releases/latest/'
   $request = [System.net.webrequest]::create($url)
   $response=$request.GetResponse()
   $OpenSSH_64bit = $([String]$response.responseuri.AbsoluteURI).Replace('tag','download') + '/OpenSSH-Win64.zip'  
   $OpenSSH_32bit = $([String]$response.responseuri.AbsoluteURI).Replace('tag','download') + '/OpenSSH-Win32.zip'
   
   # Download the desired file
   # Pick the bitness that your system needs, and update the line below if needed
   
   $OpenSSHTempPath = "$env:temp\OpenSSH_Win64.zip
   Invoke-WebRequest -uri $OpenSSH_64bit -outfile $OpenSSHTempPath
   
   # Extract the files to Program Files on your system
   
   Extract-Archive -path $OpenSSHTempPath -destination "$env:ProgramFiles\OpenSSH
   
   # Cleanup the downloaded zip file
   
   Remove-item $OpenSSHTempPath -force
   
   # Install OpenSSH
   # Note that if you downloaded a different bitness, you may need to adjust this line
   
   & "$env:programfiles\OpenSSH\OpenSSH_Win64\install.ps1"
   
   ```
   
   It's a little longer than installing the Windows Capability, but this will consistently allow you to enable OpenSSH on just about any Windows device you come across, regardless of version. This also may be a little more friendly if using a DSC or automated back-end process.
   
## Connecting after installation

Key pairs confused me at first, so let me explain them in the manner that makes sense to me. The **public key** is stored on the remote server, just like the door to a PO Box has a lock on it. Anybody with a key can attempt to open the lock on that PO box, but only someone with the matching **private key** can open the box and get the mail inside. You can add multiple private keys to your **keystore**, if you have multiple resources to access. This effectively builds a keychain on your machine. Each time you attempt to connect to a server, you'll cycle through the keys on your keychain until you find one that works or you run out of keys.

Now that OpenSSH is installed, a key pair needs to be created. This can be done using the `ssh-keygen` utility that is included in the installation of OpenSSH Server. `ssh-keygen` by default will create a key pair using RSA encryption, which is not good. In my testing, a connection cannot be established using RSA keys using this version of OpenSSH. Instead, ED25519 encryption can be specified during the key creation process.

```powershell
ssh-keygen -t ed25519 -C 'OpenSSH key using ED25519'
```

The `-C` option is used to add a comment to the key pair. If you do not specify this, the comment will default to `<username>@<hostname>`.

Now that the keys are created, we
