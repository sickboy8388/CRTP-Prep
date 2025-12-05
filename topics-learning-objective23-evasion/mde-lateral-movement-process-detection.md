# MDE - Lateral Movement - Process Detection

* Ottenuto accesso remoto utilizzare comandi come whoami.exe per l'enumerazione puo' provocare detection, in quanto e' molto improbabile come comando da usare con sqlservr.exe
* Un modo opsec friendly e' quello di utilizzare alternative come SET USERNAME che fornisce le stesse informazioni
* Esempio di detection di whoami.exe spawnate sotto sqlservr.exe

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
