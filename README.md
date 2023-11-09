# Map-Server-Relationships

>Enumeration Connected Windows Servers.

>Run from a windows domain joined computer or using compromised domain credentials.

>PowerShell script to list all computers in Active Directory contain certain string:

```powershell
Get-ADComputer -Filter * | Where-Object {$_.Name -like "*edw*"} | Select -Property Name | Out-File EDW-computers-input.txt
```

>Cleanup the output and use sysinternals tool psexec to execute netstat to list all computers with active connections to the remote targets.
>This will provide output file list all connections to each server in the input list.

>Sample input list:  

```
prd-edwserver01
dev-edwserver02
tst-edwserver03
```

>Use poweshell to validate the IP address, and then inport into Microsoft excel to cleanup the data and columns:

```powershell
$computers = Get-Content -Path "D:\scripts\EDW-computers-input.txt"
$computers.count

foreach ($computer in $computers) {
    Test-Connection $computer -count 1 -ErrorAction SilentlyContinue | FT Address,IPV4Address -hidetableheaders | Out-File "c:\users\$env:username\Documents\EDW-ip-addresses.txt" -Append
}
```  
