# 8-Powershell - Tradecraft

* Offensive PowerShell non e' morto&#x20;
* Le detection dipendono dall'utilizzo di codice custom e le relative configurazioni nel dominio target
* Esistono bypass ed esistono bypass offuscati

#### Bypassing Powershell Security

* Utilzzo di Invisi-Shell per il bypass dei controlli di sicurezza presenti

{% embed url="https://github.com/OmerYa/Invisi-Shell" %}

#### Hooking

Lo strumento modifica/intercetta due librerie fondamentali di .NET:

* **System.Management.Automation.dll** → il cuore di PowerShell
* **System.Core.dll** → libreria base di .NET

**Come funziona l'hooking:**

1. Lo strumento si "attacca" (hook) a queste DLL in memoria
2. Intercetta le chiamate alle funzioni di logging
3. Le disabilita o le reindirizza prima che possano scrivere nei log
4. Risultato: PowerShell gira ma non lascia tracce nei log

**Perché è efficace:**

* Bypassa Script Block Logging
* Bypassa System-wide Transcription
* Evita che le attività vengano registrate negli Event Logs
* Tutto avviene in memoria, senza modificare file su disco

#### CLR Profile API usato per l'hook

**Cos'è il CLR Profiler API:**

* È un'interfaccia LEGITTIMA di .NET pensata per il debugging e l'analisi delle performance
* Permette di "monitorare" cosa succede all'interno del runtime .NET

**Come viene abusato per il bypass:**

1. **Funzionamento normale (legittimo):**
   * Developer usa il profiler per capire dove un'app è lenta
   * Il profiler "ascolta" gli eventi del CLR (creazione oggetti, chiamate funzioni, ecc.)
2. **Abuso per l'evasion:**
   * Attaccante registra una DLL malevola come "profiler"
   * Il CLR carica automaticamente questa DLL all'avvio di PowerShell
   * La DLL intercetta le chiamate di logging e le blocca
   * È tutto "legittimo" agli occhi del sistema perché usa API ufficiali Microsoft

**Variabili d'ambiente usate:**

```
COR_ENABLE_PROFILING=1
COR_PROFILER={CLSID del profiler malevolo}
COR_PROFILER_PATH=C:\path\to\malicious.dll
```

**Perché è subdolo:**

* Usa funzionalità legittime di Windows
* Difficile da rilevare perché non modifica file
* Bypassa AMSI e logging senza toccare direttamente PowerShell

{% @github-files/github-code-block url="https://github.com/OmerYa/Invisi-Shell/blob/master/InvisiShellProfier/InvisiShellProfiler.cpp" %}

{% embed url="https://learn.microsoft.com/en-us/dotnet/framework/unmanaged-api/profiling/profiling-overview" %}
