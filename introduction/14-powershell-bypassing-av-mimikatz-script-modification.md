# 14-Powershell - Bypassing AV - Mimikatz - Script Modification

* Le detection per Mimikatz sono multiple e bisogna fare i seguenti step:

1. Rimuovere i commenti di default
2. Rinominare lo script, nomi di funzioni e variabili
3. Modificare i nomi delle variabili delle call WinAPI32 che generano detection
4. Offuscare il contenuto dei PEBytes --> PowerKatz dll utilizza packers
5. Implementare una funzione inversa per i PEBytes per evitare ogni signature statica
6. Aggiunere una sandbox check per rendere inutile l'analisi dinamica delle risorse
7. Rimuovere warning PE Reflective per un output piu' pulito
8. Utilizzare comandi offuscati per l'esecuzione Invoke-MimiEx
9. Scan utilizzando Defender Check

#### Rimuovere i commenti di default

&#x20;![](<../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png>)

#### Rinominare lo script, nomi di funzioni e variabili

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Offuscare il contenuto dei PEBytes --> PowerKatz dll utilizza packers

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Implementare una funzione inversa per i PEBytes per evitare ogni signature statica

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Aggiunere una sandbox check per rendere inutile l'analisi dinamica delle risorse

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

Rimuovere warning PE Reflective per un output piu' pulito, rimuovere IntPtr&#x20;

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Utilizzare comandi offuscati per l'esecuzione Invoke-MimiEx

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Scan utilizzando Defender Check

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>
