# Priv Esc - Kerberos Delegation - Overview

* Kerberos Delegation permette di ri-utilizzare end-user credentials per accedere risorse hostata su server differenti&#x20;
* Questo e' utile in servizi multi-tier o applicazioni dove Kerberos Double Hop e' richiesto. Per esempio, un utente si autentica via web server (first hop) e il server esegue richieste verso il database server (second hop)
* L'impersonificazione utente e' l'obiettivo della Delegation

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Esistono due tipi di Delegation in Kerberos
  * _**Unconstrained Delegation**_: Permette al primo hop di richiedere accesso a qualsiasi servizio su qualsiasi host nel dominio
  * _**Constrained Delegation:**_  Permette al primo hop di richiedere accesso solo a specifici servizi che girano su specifiche macchine nel dominio. Se Kerberos non e' utilizzato come meccanismo di autenticazione nel primo hop, _**Protocol Transition**_ e' utilizzato per propagare la richiesta a Kerberos
