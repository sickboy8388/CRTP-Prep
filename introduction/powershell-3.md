# Powershell - Detections

**1. System-wide transcription (Trascrizione a livello di sistema)**

* Registra TUTTO quello che viene eseguito in PowerShell su una macchina
* Crea dei log testuali di ogni comando eseguito
* Utile per forensics e incident response
* Si attiva tramite GPO e salva i log in una cartella centralizzata

**2. Script Block Logging**

* Registra i blocchi di codice PowerShell che vengono eseguiti
* Cattura anche il codice deoffuscato (!) prima dell'esecuzione
* Va negli Event Logs di Windows (Event ID 4104)
* Fondamentale per vedere cosa fa realmente uno script anche se offuscato

**3. AMSI (AntiMalware Scan Interface)**

* Intercetta gli script PowerShell PRIMA che vengano eseguiti
* Li passa all'antivirus per l'analisi in tempo reale
* Funziona in memoria, quindi blocca anche i fileless attacks
* Problema: può essere bypassato (ci sono varie tecniche)

**4. CLM (Constrained Language Mode)**

* Modalità "limitata" di PowerShell che blocca funzioni pericolose
* Integrato con AppLocker (controlla quali app possono girare) e WDAC/Device Guard
* Impedisce l'uso di .NET reflection, COM objects, chiamate API Win32
* Rende molto più difficile eseguire payload malevoli
