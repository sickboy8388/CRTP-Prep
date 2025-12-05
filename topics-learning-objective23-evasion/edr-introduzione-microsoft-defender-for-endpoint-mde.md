# EDR Introduzione - Microsoft Defender for Endpoint (MDE)

* Nel laboratorio c'e' MDE come EDR su alcuni host (eu-sql)
* Oltre alle capabilities tipiche di un EDR, MDE colleziona e processa  "segnali comportamentali" (behavioral signals) provenienti dall'OS e gli analizza utilizzando cloud security analytics
* MDE inoltre supporta la detection basandosi sulle seguenti tecnologie:&#x20;
  * Attack Surface Reduction rules, Exploit protection, Network protection, Controlled Folder Access e Device Control
* E' possibile visitare la dashboard https://security.microsoft.com ed eseguire l'accesso con le credenziali rilasciati durante l'attivazione del laboratorio, disponibili qui [https://adlab.enterprisesecurity.io/](https://adlab.enterprisesecurity.io/)
*   L'obiettivo e' quello di rimanere undetected sia lato AV che EDR su eu-sql eseguendo:

    * Comandi SQL attraverso SQL Server link
    * Trasferimento strumenti
    * Estrazione Credenziali
    * Esfiltrazione dei dati
    * Movimento Laterale / Accesso Remoto

