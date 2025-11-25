# Domain Enumeration-GPOs

* Le configurazioni relative alle Group Policy sono contenute in un Group Policy Object (GPO)
* Un GPO e' una collezione virtuale di setting di policy, permessi di sicurezza e scope of management\* (SOM) che si possno applicare a utenti e host
* GPO puo' essere linkato ad un dominio, un sito e alle organizational units (OU)
* GPO troppo permissive possono essere abusate con vettori di attacco come Privilege Escalation, Backdoors, persistence, ecc



* Ottenere le liste delle GPO per il dominio corrente

```powershell
Get-DomainGPO
```

* Mostra le GPOs applicabili ad uno specifico utente

```powershell
Get-DomainGPO -ComputerIdentity dcorp-student1
```
