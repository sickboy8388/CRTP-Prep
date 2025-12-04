# Persistence using ACLs - AdminSDHolder

* Risiede nel container di sistema di un dominio ed e' usato per controllare i permessi - utilizzando un ACL - per alcuni gruppi built-in privilegiati ( Protected Groups)
* Il Security Descritpor Propagator (SDPROP) viene eseguito ogni ora e confronta l'ACL dei gruppi/utenti protetti con l'ACL di AdminSDHolder: eventuali differenze vengono eliminate ripristinando l'ACL dell'oggetto protetto con quella del template AdminSDHolder.

#### Protected Groups

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Exploit comuni dei Gruppi Protetti - Tutti i seguenti possono autenticarsi localmente sul Domain Controller

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
