# 12-Powershell - Bypassing AV - PowerUp - Script Modification

* Utilizzare una porzione dello script e' utile spesso
* E' possibile rimuovere parti di script che vengono rilevate ma non sono utilizzate
* Per questo basta eseguire una scan con _**DefenderCheck**_ e usare lo scritp _**ByteToLineNumber.ps1**_ nella folder _C:\AD\Tools_

#### Scan using DefenderCheck

* E' possibile notare che la parte che triggera la detection e' relativa all'offset _**0x1E721**_

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* E' possibile trovare il numero di linea della parte identificato con lo script _**ByteToLineNumber.ps1**_

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Eseguendo lo script e' possibile notare che l'offset identificato corrisponde alla linea 2640 dello script, dove troviamo una stringa in Base64

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Risalendo il codice e' possibile notare che il base64 venga utilizzato dalla funzione Write-ServiceBinary, e' possibile sia rimuovere il base64 (blank variable) o rimuovere l'intera funzione

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Rimuovendo il Base64 (sempre meglio apportare modifiche minori e testare, prima di rimuovere porzioni piu' grandi di codice) verifichiamo se cambia lato detection qualcosa

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Rescan con DefenderCheck, dopo aver rimosso tutti gli altri offset riportati ogni volta che il file veniva modificato e una scan veniva eseguita

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
