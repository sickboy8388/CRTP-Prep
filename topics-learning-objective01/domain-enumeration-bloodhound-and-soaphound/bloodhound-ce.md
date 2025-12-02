# BloodHound CE



* Raccolta dati per BloodHound legacy:

{% code fullWidth="true" %}
```powershell
C:\AD\Tools\Loader.exe -Path C:\AD\Tools\Sharphound\SharpHound.exe -args --collectionmethods All
```
{% endcode %}

* I dati raccolti possono essere uplodati attraverso l'interfaccia web dell'applicativo di BloodHoundCE
* &#x20;Ricorda l'accesso Read-Only alla WEB UI con bloodhound pre-popolato
* Per rendere l'acquisizione piu' stealth e' bene rimuovere le collection piu' rumorose come: <kbd>DCOM, RDP, PSRemote and LocalAdmin</kbd>
* Utilizza <kbd>-ExcludeDCs</kbd> per evitare detection lato Microsoft Defender for Identity (MDI)

{% code fullWidth="true" %}
```powershell
C:\AD\Tools\Loader.exe -Path C:\AD\Tools\SharpHound\SharpHound.exe -args --collectionmethods
Group,GPOLocalGroup,Session,Trusts,ACL,Container,ObjectProps,SPNTargets,CertServices --excludedcs
```
{% endcode %}

* Rimuovere  la collection <kbd>"CertServices"</kbd> quando si utilizza BloodHound Legacy
