# Logged-On Users

## <mark style="color:$info;">POWERVIEW</mark>

* Ottiene tutti gli <kbd>utenti attivi autenticati</kbd> sulla macchina ( <kbd>necessita di diritti locali amministrativi sulla macchina</kbd>, es. <kbd>local admin)</kbd>

```powershell
Get-NetLoggedon -ComputerName dcorp-adminsrv
```

* Ottiene tutti gli <kbd>utenti localmente autenticati</kbd> sulla macchina ( <kbd>necessita del remote registry attivo sulla macchina target, attivato di default dal OS del server</kbd><kbd>)</kbd>

```powershell
Get-LoggedonLocal -ComputerName dcorp-adminsrv
```

* Ottiene l'ultimo <kbd>utente autenticata</kbd> sulla macchina ( <kbd>necessita di diritti amministrativi e del remote registry attivo sulla macchina target)</kbd>

```powershell
Get-LastLoggedOn -ComputerName dcorp-adminsrv
```
