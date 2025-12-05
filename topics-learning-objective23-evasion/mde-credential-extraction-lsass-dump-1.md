# MDE - Credential Extraction - LSASS Dump 1

* Eseguire il dump delle credenziali usando LSASS in modo diretto o in termini di estrazioni dati dalla memoria del processo  (es. Mimikatz sekurlsa::logonpasswords) viene rilevato dal MDE
* Un modo piu opsec friendly prevede l'intero dump del processo usando tecninche stealth e la successiva  analisi offline
* Tuttavia tecniche standard utili al dump dell'intero processo LSASS (es. taskmanager -> Create Dump File) sono rilevate e bloccate
* La maggior parte dei tool per fare il dump del processo LSASS
  * Prendono possesso del processo
  * Creano minidump utilizzando MiniDumpWriteDump WinApi funzione implementata in _dbghelp.dll / dbgcore.dll_
* Queste 3 azioni sono fortemente monitorate dagli EDR e spesso rilevate e bloccate
  * Per evitare queste rilevazioni, si devono evitare tool che implementano MiniDumpWriteDump ed eseguire il dump in modo differente
  * MiniDumpDotNet ([https://github.com/WhiteOakSecurity/MiniDumpDotNet](https://github.com/WhiteOakSecurity/MiniDumpDotNet)) e' un tool che implementa una versione custom dell MiniDumpWriteDump della WinApi32
  * &#x20;MiniDumpDotNet Blog - https://www.whiteoaksecurity.com/blog/minidumpdotnet- part-1/
  * NanoDump Github: https://github.com/helpsystems/nanodump&#x20;
  * PostDump Github: https://github.com/YOLOP0wn/POSTDump&#x20;
  * Custom MiniDumpWriteDump BOF Github:https://github.com/rookuu/BOFs/tree/main/MiniDumpWriteDump&#x20;
  * ReactOS Source code for MiniDumpWriteDump implementation: https://doxygen.reactos.org/d8/d5d/minidump\_8c\_source.html&#x20;
  * MiniDumpWriteDump Windows API function: https://learn.microsoft.com/en- us/windows/win32/api/minidumpapiset/nf-minidumpapiset-minidumpwritedump
