# 7-Powershell - Execution Policy

* Non e' una security features, ma usato per prevenire l'esecuzione accidentale di script/eseguibili
* Diversi modi per bypassarlo

```powershell
powershell -ExecutionPolicy bypass
powershell -c <cmd>
powershell -encodedcommand
$env:PSExecutionPolicyPreference="bypass"
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://www.netspi.com/blog/entryid/238/15-ways-to-bypass-the-powershell-execution-policy" %}
