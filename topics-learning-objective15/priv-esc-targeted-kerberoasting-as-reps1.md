# Priv Esc - Targeted Kerberoasting - AS-REPs1

* Se nei setting UserAccountControl di un utente "Do not require Kerberos preauthentication" e' abilitato, significa che Kerberos pre-auth e' disabilitato, ed e' possibile recuperare lo user AS-REP ed eseguire il bruteforce offline
* Con i permessi corretti (GenericWrite o GenericAll), Kerberos preauth puo' essere disabilitato nel caso sia attivo

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
