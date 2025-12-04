# Priv Esc - Across domain trusts - AD CS - Abusing ESC3 Theory

#### Template 1: "SmartCardEnrollment-Agent"

* **Chi può richiederlo**: tutti gli utenti del dominio
* **Cosa contiene**: l'EKU (Extended Key Usage) "Certificate Request Agent"
* **Cosa significa**: questo certificato ti dà il potere di richiedere certificati **per conto di altri utenti**. È come avere una delega in bianco.

#### Template 2: "SmartCardEnrollment-Users"

* **Requisito speciale**: per ottenerlo, DEVI presentare un certificato di tipo "Certificate Request Agent" (cioè quello del Template 1)
* **Cosa contiene**: un EKU che permette l'autenticazione al dominio (tipo "Client Authentication" o "Smart Card Logon")
* **Cosa significa**: questo certificato ti permette di autenticarti come un utente del dominio

### L'Attacco in Pratica

1. **Passo 1**: Come utente normale del dominio, richiedi il certificato "SmartCardEnrollment-Agent" (puoi farlo perché è accessibile a tutti)
2. **Passo 2**: Usando quel certificato di Agent, richiedi un certificato "SmartCardEnrollment-Users" **specificando come soggetto l'Administrator** (o qualsiasi altro utente vittima)
3. **Passo 3**: Usi il certificato dell'Administrator per autenticarti al dominio come lui

