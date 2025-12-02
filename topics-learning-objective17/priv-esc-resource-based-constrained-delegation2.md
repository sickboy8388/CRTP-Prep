# Priv Esc - Resource-based Constrained Delegation2

#### Per abusare RBCD in modo efficace, abbiamo bisogno di due privilegi

* Write Permission sul servizio/oggetto target per configurare msDS-AllowedToActOnBehalfOfOtherIdentity.
* Controllo su un oggetto del dominio che abbia SPN configurati (come accesso amministrativo a macchina joinata a dominio o l'abilita di joinare una macchina a dominio - ms-DS-MachineAccountQuota is 10 for all domain users)

