# Map Server Relationships  

>Enumeration Connected Windows Servers.

>Run from a windows domain joined computer or using compromised domain credentials.

>PowerShell script to list all computers servers in Active Directory contain certain string value in their name:  

```powershell
Get-ADComputer -Filter * -Properties LastLogonDate | 
    Where-Object { $_.Name -like "*sas*" } | 
    Select-Object Name, LastLogonDate | 
    Export-Csv -Path "D:\Support\servers\sas-servers-list.csv" -NoTypeInformation -Force

```  

>Cleanup the output and use sysinternals tool psexec to execute netstat to list all computers with active connections to the remote targets.
>This will provide output file list all connections to each server in the input list.

>Sample input list:  

```
prd-edwserver01
dev-edwserver02
tst-edwserver03
```

>Use input list to get the netstat output using below psexec:

```
PsExec.exe @EDW-computers-input.txt netstat -af >EDW-output-netstat-results.txt
```  

>Cleanup the `EDW-output-netstat-results.txt` by removing duplicates and making a column of DNS server names to use as input file `\EDW-computers-input.txt` to validate IP address.

>Use poweshell to validate the IP address, and then open output into Microsoft Excel to cleanup the data and columns:

```powershell
$computers = Get-Content -Path "D:\scripts\EDW-computers-input.txt"
$computers.count

foreach ($computer in $computers) {
    Test-Connection $computer -count 1 -ErrorAction SilentlyContinue | FT Address,IPV4Address -hidetableheaders | Out-File "c:\users\$env:username\Documents\EDW-ip-addresses.txt" -Append
}
```  

## Attack Surface  

>The above give view of the attack surface based on the discovered system connections.

![/attack%20surface.PNG](/attack%20surface.PNG)  

----  
