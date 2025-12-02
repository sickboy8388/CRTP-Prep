# Privilege Escalation - GPO Abuse - GPOddity

* GPOddity combina NTLM Relaying e modifiche al Group Policy Container
* Facendo Relaying rispetto alle credenziali di un utente che ha WriteDACL su GPO e'possibile modificare il path ( con cPCFileSysPath) del Group Policy template (default is SYSVOL)
* Questo permette il caricamento di template malevoli da una locazione sotto il nostro controllo

<figure><img src="../.gitbook/assets/Schermata da 2025-11-26 14-53-01.png" alt=""><figcaption></figcaption></figure>
