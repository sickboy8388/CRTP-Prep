# MDE - Breaking Detection Chains

* Gli EDR correlanto attivita' anche su basi temporali, questo varia da EDR a EDR e rispettive configurazioni
* Per Bypassare tale rilevazioni e' possibile
  * Tentare di attendere piccoli intervalli di tempo (\~10 mins) prima di eseguire una nuova query
  * Appendere query non sospette all'interno di esecuzioni di query sospette, eseguendo semplici query sul server eu-sql
