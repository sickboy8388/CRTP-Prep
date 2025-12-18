# 2-Active Directory

* Directory Service utilizzato per gestire reti Windows
* Mantiene informazioni relative agli oggetti (host/user) della rete e li rende disponibili per utenti e amminstratori
* Active Directory abilita la gestione sicura centralizzata di una rete intera, che puo' essere distribuito all'interno di un palazzo, una citta' o locazioni multiple&#x20;

{% embed url="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)" %}

{% embed url="https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview" %}

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)" %}

#### Componenti AD

* _**Schema:**_ Definisce gli oggetti del dominio e i relativi attributi
* _**Meccanismi di querying e indicizzazione:**_ Fornisco ricerca e ottenimento degli oggetti e delle relative proprieta'
* _**Catalogo Globale:**_ Contiene informazioni rispetto ad ogni oggetto nel dominio
* _**Replication Service:**_ Distribuisce le informazioni tra Domain Controllers (DC) all'interno del dominio

#### Struttura AD

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Foreste, Domini e Organizational Units (OUs) sono gli elementi di base di un AD
* Una foresta, che rappresenta un security boundary, che puo' contenere domini multipli e ogni dominio multiple OUs
