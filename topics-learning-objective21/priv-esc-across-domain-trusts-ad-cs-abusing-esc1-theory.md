# Priv Esc - Across domain trusts - AD CS - Abusing ESC1 Theory

### Il Problema (ESC1 - Subject Alternative Name Attack)

Hai un template di certificato configurato male che ti permette di **specificare TU chi sei** nel certificato, invece che farlo decidere al sistema.

#### Template: "HTTPSCertificates"

Ha il flag `ENROLLEE_SUPPLIES_SUBJECT` attivo nel campo `msPKI-Certificates-Name-Flag`

**Cosa significa**: Quando richiedi il certificato, **tu puoi scrivere nel campo Subject (o Subject Alternative Name) qualsiasi identità tu voglia**. È come compilare da solo il tuo documento d'identità e la CA (Certificate Authority) te lo firma senza verificare.

**Chi può richiederlo**: Il gruppo "RDPUsers" (di cui fai parte come studentx)

**Cosa contiene**: Presumibilmente un EKU che permette l'autenticazione (tipo Client Authentication/Smart Card Logon)
