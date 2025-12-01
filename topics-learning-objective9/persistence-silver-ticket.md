# Persistence - Silver Ticket

* Un ticket Service valido o TGS (TGT=Golden Ticket, TGS=Silver Ticket)
* Firmato e cifrato con l'hash del service account (Golden Ticket e' firmato con l'hash del krbtgt) del servizio eseguito con quell'account.
* I servizi raramente vengono verificato con PAC (Privileged Attribute Certificate)
* Il Silver Ticket garantisce accesso solo al servizio specifico per cui è stato forgiato, non ad altri servizi o risorse del dominio.
* Periodo di persistenza ragionevole ( 30 giorni per i computer account)&#x20;

