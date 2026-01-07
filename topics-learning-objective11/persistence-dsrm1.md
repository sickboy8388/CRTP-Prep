# Persistence - DSRM1

* DSRM e' la Directory Service Restore Mode
* C'e' un local administrator su ogni DC chiamato "Administrator" la cui password e' la password el DSRM
* DSRM password (SafeModePassword) e' richiesta quando un server viene eletto/promosso a DC e raramente viene cambiata
* Dopo aver modificato la configurazione sul DC, e' possibile usare l'hash NTLM dello user per accedere al DC

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
