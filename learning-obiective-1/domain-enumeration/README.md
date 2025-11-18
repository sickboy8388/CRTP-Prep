# Domain Enumeration

### _**Getting Started**_

I tool utilizzati per la fase di enumeration sono i seguenti:

* The ActiveDirectory PowerShell module ( firmato MS e funzionante anche con Powershell CLM)

&#x20;     [https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps](https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps)\
&#x20;     [https://github.com/samratashok/ADModule](https://github.com/samratashok/ADModule)

\
&#x20;     `Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll`\
&#x20;     `Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1`

&#x20;

* &#x20;BloodHound (C# and PowerShell Collectors)\
  [https://github.com/BloodHoundAD/BloodHound](https://github.com/BloodHoundAD/BloodHound)



* PowerView (PowerShell)\
  [https://github.com/ZeroDayLab/PowerSploit/blob/master/Recon/PowerView.ps1](https://github.com/ZeroDayLab/PowerSploit/blob/master/Recon/PowerView.ps1)\
  `. C:\AD\Tools\PowerView.ps1`



* &#x20;SharpView (C#) - Doesn't support filtering

&#x20;      [https://github.com/tevora-threat/SharpView/](https://github.com/tevora-threat/SharpView/)



_**Extras:**_ [_**https://janikvonrotz.ch/2015/09/09/deploy-powershell-activedirectory-module-**_\
_**without-installing-the-remote-server-tools/**_\
_**https://www.labofapenetrationtester.com/2018/10/domain-enumeration-from-**_\
_**PowerShell-CLM.html**_](https://janikvonrotz.ch/2015/09/09/deploy-powershell-activedirectory-module-without-installing-the-remote-server-tools/https://www.labofapenetrationtester.com/2018/10/domain-enumeration-from-PowerShell-CLM.html)



### Domain Enumeration

\
\
\
