# Local Groups

## <mark style="color:$info;">POWERVIEW</mark>

* Lista tutti i <kbd>gruppi locali</kbd> presenti sulla macchina (<kbd>necessita' di privilegi amministrativi su macchine non-dc, Local Admin</kbd>)

```powershell
Get-NetLocalGroup -ComputerName dcorp-dc
```

* Ottiene i <kbd>membri</kbd> del <kbd>gruppo locale "Administrator"</kbd> della macchina (<kbd>necessita' di privilegi amministrativi su macchine non-dc, Local Admin</kbd>)

```powershell
Get-NetLocalGroupMember -ComputerName dcorp-dc -GroupName
Administrators
```

