# Persistence - Diamond Ticket

* Un Diamond Ticket e' creato a partire da  un TGT valido decifrato, modificato e ri-cifrato utilizzando l'AES key del krbtgt account
* Golden Ticket e' un TGT forging attacks, in quanto il ticket e' "forgiato" da zero, mentre Il Diamond Ticket riguarda e' TGT modification attacks in quanto il ticket viene modificato dall'attaccante
* La persistenza in termini di lifetime dipende sempre dal krbtgt account
* Un Diamond Ticket e' piu' OPSEC poiche':
  * parte da un TGT reale emesso dal DC che viene modificato, mantenendo timestamp e sequenze valide nei log.
  * Nel Golden Ticket, manca la richiesta TGT iniziale nei log, creando un'anomalia: appaiono richieste TGS/Service ticket senza il corrispondente TGT.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
