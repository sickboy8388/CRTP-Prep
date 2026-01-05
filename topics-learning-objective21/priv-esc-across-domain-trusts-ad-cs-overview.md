# Priv Esc - Across domain trusts - AD CS - Overview

* Active Directory Certificate Services (AD CS) abilita l'utilizza della Public Key Infrastracture nella foresta Active Directory
* AD CS aiuta nel processo di autenticazione degli utenti e delle macchine nel dominio, cifra e firma documenti, filesystem, email e altro
* Elementi della PKI di AD CS
  * CA - La certification authority che emette i certificati. Il server con il ruole AD CS  ( in genere un DC o un altro server nel dominio) e' la CA
  * Certififcato - Emesso per conto di un utente o di una macchina e puo essere usato per autenticazione, cifratura e firma dei messaggi, ecc
  * CSR - Certificate Signing Request fatto da un client verso la CA al fine di richiedere un certificato
  * Certificate Template - Definisce i setting del certificato, contiene informazioni come, permessi di enrollmente, EKUs, scadenza, ecc.
  * EKU OIDs - Extended Key Usage Object Identifiers, determina l'uso finale di un\
    certificate template (autenticazione client, accesso con smart card, SubCA ecc.)

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Esistono diversi metodi per l'abuso di AD CS ([https://specterops.io/wp-content/uploads/sites/3/2022/06/Certified\_Pre-Owned.pdf](https://specterops.io/wp-content/uploads/sites/3/2022/06/Certified_Pre-Owned.pdf))
  * Estrazione certificati utenti e macchine
  * Utilizza i certificati per recuperare gli hash NTLM
  * Persistenza a livello utente e macchina
  * Escalation a Domain Admin e Enterprise Admin
  * Domain persistence
* NON SARANNO DISCUSSE TUTTE LE TECNICHE

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
