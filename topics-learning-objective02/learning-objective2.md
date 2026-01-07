# LEARNING OBJECTIVE2

<div data-full-width="true"><figure><img src="../.gitbook/assets/learing2 (2).png" alt=""><figcaption></figcaption></figure></div>

* Enumerare le seguenti <kbd>ACL</kbd> per il dominio <kbd>dollarcorp</kbd>:
  * <kbd>ACL</kbd> per il gruppo <kbd>Domain Admins</kbd> for the Domain Admins group
  * &#x20;<kbd>ACL</kbd> dove lo <kbd>studentex</kbd> ha permessi interessanti&#x20;
* Analizzare i permessi dello <kbd>studentex</kbd> in <kbd>BloodHound UI</kbd>&#x20;

#### Enumerazione ACL Domain Admins Group

{% code fullWidth="true" %}
```powershell
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -
Verbose
```
{% endcode %}

#### Enumerazione ACL student178

{% code fullWidth="true" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "studentx"}
```
{% endcode %}

#### Enumerazione ACL RDPUsers per Control178User

{% code fullWidth="true" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "studentx"}
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## FLAG: GenericAll
