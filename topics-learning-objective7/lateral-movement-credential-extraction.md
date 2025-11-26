# Lateral Movement - Credential Extraction

* La Local Security Authority (LSA) e' responsabile dell'autenticazione sulle Windwos machine. Local Security Authority Subsystem Service (LSASS) e' il servizio relativo
* LSASS esegue lo storing delle credenziali in forme differenti - NT Hash, AES, Kerberos tickets e altro
* Le credenziali vengono storate da LSASS quando un utente:
  * Esegue un logon via RDP o tramite sessione locale
  * Utilizza RunAs
  * Esegue un Windows Service
  * Utilizza un tool di amministrazione remota
* Il processo LSASS e' per questo motivo un target molto appetibile
* E' anche il processo piu' monitorato su una macchina windows
* Alcune delle credenziali che possono essere estratte senza toccare LSASS
  * SAM hive (Registry) - Credenziali Locali
  * LSA Secrets/SECURITY hive (Registry) - Password dei service account, credenziali di dominio cached, ecc
  * DPAPI Protected Credentials (Disk) - Credentials Manager/Vault, Cookie dei browser, Certificati, token Azure, ecc

