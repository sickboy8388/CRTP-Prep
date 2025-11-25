# Shares e File

## <mark style="color:$info;">POWERVIEW</mark>

* Trova le <kbd>share presenti</kbd> sugli host del dominio a cui si appartiene

```powershell
Invoke-ShareFinder -Verbose
```

* Trova <kbd>file sensibili</kbd> sugli host del dominio afferente

```powershell
Invoke-FileFinder -Verbose
```

* Ottiene tutti i <kbd>fileserver</kbd> presenti nel dominio

```powershell
Get-NetFileServer
```

## <mark style="color:green;">POWER HUNTER SHARES</mark>

* Per le enumerazione delle share un altro tool utile e' PowerHuntShares disponibile qui: [https://github.com/NetSPI/PowerHuntShares](https://github.com/NetSPI/PowerHuntShares)

Identifica share, file sensibili, ACL per le share, networks, host, identita',ecca e genera inoltre un report HTML

<pre class="language-powershell" data-full-width="true"><code class="lang-powershell"><strong>Invoke-HuntSMBShares -NoPing -OutputDirectory C:\AD\Tools -HostList C:\AD\Tools\servers.txt
</strong></code></pre>

* Il file 'servers.txt' nel comando non deve includere il DC per un OPSEC maggiore
* Il report genera inoltre lo 'ShareGraph' che puo' essere utilizzato per analizzare le relazioni delle share&#x20;
