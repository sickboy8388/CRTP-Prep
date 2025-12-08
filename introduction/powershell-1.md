# Powershell - Scripts e Moduli

* Caricare un modello PS utilizzanto il _**"dot sourcing"**_

```powershell
. C:\AD\Tools\PowerView.ps1
```

* Import di un modulo o di uno script

```powershell
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory.psd1
```

* Listing di tutti i comandi di un modulo

```powershell
Get-Command -Module <modulename>
```
