# Persistence - Golden Ticket

* Un <kbd>golden ticket</kbd> e' un hash firmato e cifrato dall'account <kbd>krbtgt</kbd>  che lo rende un valido ticket <kbd>TGT</kbd>
* L'hash dello user krbtgt puo' essere usato per impersonificare qualsiasi utente con qualsiasi privilegi persino da macchine non in dominio
* Come buona pratica, si raccomanda di cambiare la password dell'account\
  krbtgt due volte, poiché per l'account viene mantenuta la cronologia delle password.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
