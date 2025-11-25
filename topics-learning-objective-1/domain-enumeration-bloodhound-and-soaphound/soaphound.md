# SOAPHound

* SoapHound risulta essere piu' stealth
* Utilizza Active Directory Web Services (ADWS - TCP Port 9389) invece di inviare LDAP queries
  * Network detection con MDI molto piu' bassa
  * RItorna informazioni rispetto tutti gli oggetti del dominio (objectGuid=\*) e li processa, cio' limita considerevolmente le query LDAP con conseguente minore probabilita' in termini di enpoint detection
* Costruisce una cache che include le info di base rispetto agli oggetti di dominio

```powershell
SOAPHound.exe --buildcache -c C:\AD\Tools\cache.txt
```

* Colleziona un set compatibile per BloodHound

```powershell
SOAPHound.exe -c C:\AD\Tools\cache.txt --bhdump -o C:\AD\Tools\bloodhound-output --nolaps
```
