# Priv Esc - Unconstrained Delegation - Overview

* Peremette la delega ad ogni servizio e risorsa del dominio come uente
* Quando e' abilitata, il Domain Controller include il TGT dell'utente all'interno del TGS. Nel primo hop, il TGT e' estratto dal TGS e salvato nel LSASS. In questo modo il server puo' riutilizzare il TGT per accedere ogni altra risorse
* Questo e' un invito all abuso (LOL !)

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
