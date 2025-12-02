# Lateral Movement - PowerShell Remoting - Tradecraft

* PowerShell Remoting supporta sia i transcript system-wide per i log che per i vari scritpblock
* E' possibile utilizzare winrs invece di PSRemoting per evadere il logging (e beneficiare ancora della 5985 tra gli host)

```powershell
winrs -remote:server1 -u:server1\administrator -p:Pass@1234 hostname    
```

* E' possibile utilizzare winrm.vbs e oggetti COM di WSMan --> [https://github.com/bohops/WSMan-WinRM](https://github.com/bohops/WSMan-WinRM)
