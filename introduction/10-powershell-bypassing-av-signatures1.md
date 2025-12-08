# 10-Powershell - Bypassing AV Signatures1

* Possiamo sempre caricare script in memoria al fine di evitare detection utilzzando AMSI bypass
* Utilizziamo AMSITrigger ([https://github.com/RythmStick/AMSITrigger](https://github.com/RythmStick/AMSITrigger)) o DefenderCheck([https://github.com/t3hbb/DefenderCheck](https://github.com/t3hbb/DefenderCheck)) per identificare codice/stringhe presenti nel binario che possono far scattare detection lato Windows Defender
* Basta fornire lo script su cui eseguire l'analisi

```powershell
AmsiTrigger_x64.exe -i C:\AD\Tools\Invoke-PowerShellTcp_Detected.ps1
DefenderCheck.exe PowerUp.ps1
```

Per una offuscazione completa utilizzare Invoke-Obfuscation ([https://github.com/danielbohannon/Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation)) utilizzato per offuscare l'AMSI bypass nel corso

{% embed url="https://github.com/t3l3machus/PowerShell-Obfuscation-Bible" %}
