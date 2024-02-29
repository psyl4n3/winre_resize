# winre_resize
Modified Microsoft script to resize a WinRE partition for applying KB5034441

## Readme

**Background:** In managing a fleet of endpoints that includes Windows 10 endpoints, I began to experience the known 0x80070643 failure. Microsoft has provided a PowerShell script to resize the WinRE partition to allow the patch to install. That script can be found at [https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-update-to-winre?view=windows-11#extend-the-windows-re-partition](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-update-to-winre?view=windows-11#extend-the-windows-re-partition)

The issue that I found with this script as is when deploying via automation lies in the SkipConfirmation process and passing the -SkipConfirmation $true flag per Microsoft's instructions. 


> C:\WINDOWS\system32\config\systemprofile\AppData\Local\gywftbfd.utu.ps1 : Cannot process argument transformation on 
>parameter 'SkipConfirmation'. Cannot convert value "System.String" to type "System.Boolean". Boolean parameters accept 
>only Boolean values and numbers, such as $True, $False, 1 or 0.
>    + CategoryInfo          : InvalidData: (:) [gywftbfd.utu.ps1], ParentContainsErrorRecordException
>    + FullyQualifiedErrorId : ParameterArgumentTransformationError,gywftbfd.utu.ps1

This script has been slightly modified by commenting out the SkipConfirmation section and replacing code that makes the script always skip confirmation at execution.

The following command will execute this script assuming you've saved it to C:\Resize_script.ps1 and created a backup folder at C:\winre_backup

```
powershell.exe -ExecutionPolicy Bypass -File "C:\Resize_script.ps1" -BackupFolder "c:\winre_backup"
```

From an automation standpoint, executing this at scale would be the following process:

- Deliver Resize_script.ps1 to C:\
- Create the folder C:\winre_backup
- Force the machine to reboot
- Pass the above command as an admin
- **Optional** Delete Resize_script.ps1 for housekeeping purposes
