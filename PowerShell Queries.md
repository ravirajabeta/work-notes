# PowerShell Queries for SCCM / MECM / Intune

**<ins>Collect the reboot count for a device over the last 7 days :</ins>**
```ps
(Get-WinEvent -FilterHashtable @{LogName='System';ID=1074;StartTime=(Get-Date).AddDays(-7)} -ErrorAction SilentlyContinue) | ForEach-Object {$script:count++; $_} | Select-Object TimeCreated,Message | Format-Table -AutoSize; Write-Host "Reboot Count: $script:count"
```
