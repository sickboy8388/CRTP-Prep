# Priv Esc - Across Trusts

* Tra Domini: Trust bidirezionale implicito
* Tra Foreste: Trust va stabilito

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* sIDHistory e' un attributo dell'utente utilizzato in scenari come quello in cui un utente viene spostato da un dominio a un altro. Al cambio del dominio un nuovo sID viene associato all'utente e il vecchio viene aggiunto al sIDHistory
*   sIDHistory puo' essere abusato in 2 modi per scalare i privilegi in una foresta (Enterprise Admins)

    * krbtgt hash del figlio&#x20;
    * Trust tickets



#### KERBEROS  - ACROSS DOMAIN TRUSTS

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Priv Esc - Enterprise Admins - Trust Key Abuse

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>
