# Privilege Escalation-Local

* Ci sono diversi modi per scalare localmente i privilegi su una box Windows:
  * Patch di sicurezza mancanti
  * Deployment automatizzati e password per AutoLogon in chiaro
  * AlwaysInstalledElevated (Ogni utente puo' usare MSI con privilegi SYSTEM)
  * Misconfigured Services
  * DLL Hijiackin and more
  * Kerberos and NTLM Relaying
* E' possibile utilizzare i tool sottostanti per una compertua completa rispetto all'argomento
  * PoweUp: [https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc)
  * Privesc: [https://github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck)
  * winPEAS: [https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)
* Esegui tutti i check
  * **PowerUP** --> `Invoke-AllChecks`&#x20;
  * **Privesc** --> `Invoke-PrivEscCheck`
  * **PEASS-ng** --> `winPEASx64.exe`

