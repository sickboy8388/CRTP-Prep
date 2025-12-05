# MDE - Lateral Movement - ASR Rules

* MDE correlano rilevzazioni rispetto alle regole della Attack Surface Reduction (ASR)
* Le regole ASR sono configurazioni che possono essere customizzate e applicate per ridurre la superfice di attacco di una macchina. Queste regole posso essere customizzate e referenziate con specifici GUIDs
* Le regola ASR sono scritte in .lua e possono essere reversate ed estratte da un target
* Sono semplici da capire, _GetMonitoredLocations_ mostra i processi che sono monitorati e l'esecuzione remota su questi processi risulta in una detection

"Block process creations originating from PSExec and WMI commands“ ASR rule\
reversed: [https://github.com/HackingLZ/ExtractedDefender/blob/main/asr/d1e49aac-8f56-4280-b9ba-993a6d77406c](https://github.com/HackingLZ/ExtractedDefender/blob/main/asr/d1e49aac-8f56-4280-b9ba-993a6d77406c)

* Per evitare detection basate su ASR specifiche come "Block process creations originating from PSExec and WMI commands"&#x20;
  * Usare alternative come winrm per l'accesso (winrs) invece d PSExec/WMI (Non rilevato da MDE ma rilevato da MDI)
  * Utilzzare _GetCommandLineExclusions_ che mostra la lista di esclusioni in termini di command line (es. ".:\windows\ccm\systemtemp\\.+) se incluso nella command line risultera' in un bypass della regola e della successiva detection

```powershell
C:\AD\Tools\WSManWinRM.exe eu-sql.eu.eurocorp.local "cmd /c notepad.exe C:\Windows\ccm\systemtemp\"
```

