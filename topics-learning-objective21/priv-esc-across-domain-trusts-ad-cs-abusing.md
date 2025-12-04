# Priv Esc - Across domain trusts - AD CS - Abusing

* E' possibile utilizzare Certify tool ( [https://github.com/GhostPack/Certify](https://github.com/GhostPack/Certify) ) per enumerare AD CS  nella foresta target (puo' essere usato per altri attacchi)

```powershell
Certify.exe cas
```

* Enumerare i template

```powershell
Certify.exe find
```

* Enumerare i template vulnerabili

```powershell
Certify.exe find /vulnerable
```

Nel lab moneycorp, ci sono diverse misconfigurations in AD CS

* Le misconfigurations presenti sono tutti riconducibili a ESC1 e ESC3
  * CA garantisce permessi di enrollment per utenti normali/non privilegiati
  * L'approvazione manager e' disabilitata
  * Le firme per l'autorizzazione non sono richieste
  * Il template target garantisce agli utenti normali/con privilegi limitati i permessi di enrollment.
