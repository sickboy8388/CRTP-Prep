# Priv Esc - Kerberoasting - John The Ripper

* Rompere gli hash con John The Ripper

```powershell
john.exe --wordlist=C:\AD\Tools\kerberoast\10k-worst-pass.txt C:\AD\Tools\hashes.txt
```

* Utilizzare tool come crunch, bopscrk, CeWL per generare wordlist custom
  * [https://github.com/jim3ma/crunch\
    https://github.com/r3nt0n/bopscrk\
    https://github.com/digininja/CeWL](https://github.com/jim3ma/crunchhttps://github.com/r3nt0n/bopscrkhttps://github.com/digininja/CeWL)
* Un'altra ottima collezione di wordlist puo' essere trovata qui:
  * [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
