# Priv Esc - Constrained Delegation with Protocol Transition - Overview

* Permette l'accesso solo a servizi specifici su host specifici
* Per impersonare l'utente e' utilizzata l'estenzione _**Service for User (S4U)**_, che fornisce due sotto-estenzioni:
  * _**Service for User to Self (S4U2Self):**_ Permette ad un servizio di ottenere un <kbd>TGS forwardabile</kbd> a se stesso, per conto utente, richiedendo solo lo user prinical name senza richiedere una password
  * _**Service for User to Proxy (S4U2Proxy):**_ Permette ad un servizio di ottenere un <kbd>TGS forwardabile</kbd> a un secondo servizio per conto utente. I servizi che permettono cio' sono controllati dall'attributo <kbd>msDS-AllowedToDelegateTo</kbd> che contiene <kbd>una lista di SPN</kbd> a cui il token puo' essere forwardato&#x20;

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Per poter abusare lo scenario descritto sopra, abbiamo bisogno dell'accesso al websvc account. Se abbiamo accesso a quel account, e' possibile accedere a tutti i servizi listati in msDS-AllowedToDelegateTO
