# Priv Esc - Kerberoasting

* Eseguire cracking offline di password di service account
* Il Kerberos session ticket (TGS Ticket Granted Service) ha un Il ticket di sessione Kerberos (TGS) ha una parte che e' cifrata l'hash della password dell'account di servizio. Ciò rende\
  possibile richiedere un ticket ed eseguire un attacco offline alla password.
* Poiche (non-machine) le password dei service account non sono cambiate molto spesso questa tipologia di attacco e' fortemente diffuso

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
