---
layout: post
title: Powershells
category: Tools
tag: imported
author: adricnet
---
<em>A few examples from the major Windows command line tools</em>

```
### c:\tools\504>help for | findstr "FOR" > helpfor-examples.txt
FOR %variable IN (set) DO command [command-parameters]
To use the FOR command in a batch program, specify %%variable instead
forms of the FOR command are supported:
FOR /D %variable IN (set) DO command [command-parameters]
FOR /R [[drive:]path] %variable IN (set) DO command [command-parameters]
    Walks the directory tree rooted at [drive:]path, executing the FOR
FOR /L %variable IN (start,step,end) DO command [command-parameters]
FOR /F ["options"] %variable IN (file-set) DO command [command-parameters]
FOR /F ["options"] %variable IN ("string") DO command [command-parameters]
FOR /F ["options"] %variable IN ('command') DO command [command-parameters]
FOR /F ["options"] %variable IN (file-set) DO command [command-parameters]
FOR /F ["options"] %variable IN ('string') DO command [command-parameters]
FOR /F ["options"] %variable IN (`command`) DO command [command-parameters]
FOR /F "eol=; tokens=2,3* delims=, " %i in (myfile.txt) do @echo %i %j %k
    Remember, FOR variables are single-letter, case sensitive, global,
    You can also use the FOR /F parsing logic on an immediate string, by
    Finally, you can use the FOR /F command to parse the output of a
      FOR /F "usebackq delims==" %i IN (`set`) DO @echo %i
In addition, substitution of FOR variable references has been enhanced.
values.  The %~ syntax is terminated by a valid FOR variable name.
```

```
netsh interface ipv4>set address name="VMware Network Adapter VMnet1" static addr=192.168.116.1 mask=255.255.255.0
```

--- old ---

<em>A few examples from the major Windows command line tools</em>

<h2>netsh tricks</h2>

```
netsh interface show interface

netsh advfirewall set allprofiles state on

netsh advfirewall set allprofiles state off

netsh advfirewall set currentprofile firewallpolicy blockinbound, allowoutbound

netsh dhcpclient trace enable

netsh wlan connect ssid="Wireless Net" name=Profile2 interface="Wireless Network Connection"
```

<h2>Powershell and wmic</h2>
<h3>Help</h3>

```
help

get-help

get-help services

get-help stop-services
```

<h3>files and reg keys</h3>

```
Test-Path C:\ ##tab

Test-Path HKCU:\Software\Microsoft\Windows\CurrentVersion
```


<h3>Service info</h3>

```
wmic service list brief

wmic service get /format:hform &gt; c:\folder\services.html

start services.html

wmic /node:steve-pc service list brief
```

<h3>Service manipulation</h3>

```
Suspend-Service tapisrv

Suspend-Service -displayname "telephony"

Resume-service tapisrv
```

<h3>Processes</h3>

```
get-help start-process

start notepad.exe

start-process iexplore

Stop-Process -processname iexplore

Stop-Process -processname note*
```

<h3>Bios and Hardware</h3>

```
wmic bios

Get-WmiObject Win32_Share | Select Name, Path, Type | FT

Get-WmiObject Win32_LogicalDisk | Select Name, Size, FreeSpace

driverquery.exe /v /FO CSV | ConvertFrom-CSV | Select-Object 'Display Name', 'Start Mode', 'Paged Pool(bytes)', Path

```

<h2>Refs</h2>
Command Line Kung fu blog:
[link](http://blog.commandlinekungfu.com/p/index-of-tips-and-tricks.html)

netsh Tech Reference: (http://technet.microsoft.com/en-us/library/cc725935(v=ws.10).aspx)

Learn Windows PowerShell Fast Crash Course:
[link](http://www.trainsignal.com/blog/learn-windows-powershell-fast-crash-course)

Task Based Guide Powershell cmdlets: (http://technet.microsoft.com/en-us/library/ee332526.aspx)

[link](http://www.softwarecrew.com/2011/01/wmic-the-best-command-line-tool-youve-never-used/)

TN Blogs: "Using PowerShell V2 to gather info on free space on the volumes of your remote file server":
[link](http://blogs.technet.com/b/josebda/archive/2010/04/08/using-powershell-v2-to-gather-info-on-free-space-on-the-volumes-of-your-remote-file-server.aspx)

Finding Driver Information using PowerShell, a PowerTip of the Day, from PowerShell.com:
[link](http://newdelhipowershellusergroup.blogspot.com/2012/01/finding-driver-information-using.html)
