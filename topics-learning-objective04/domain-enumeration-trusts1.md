# Domain Enumeration - Trusts1

* In un ambiente AD, per <kbd>trust</kbd> si intende la <kbd>relazione tra due domini o foreste</kbd>, che <kbd>permette agli utenti</kbd> di un dominio o di una foresta <kbd>di accedere risorse in altri domini o foreste</kbd>
* Il <kbd>Trust</kbd> puo' essere <kbd>automatico</kbd> (padre-figlio, stessa foresta) o <kbd>stabilito</kbd> (foresta, dominio esterno)
* I <kbd>Trusted Domain Objects (TDOs)</kbd> rappresentano le <kbd>relazioni</kbd> di <kbd>trust</kbd> in un <kbd>domino</kbd>

### Direzione Trust

* **One-way-trust**, unidirezionale, gli utenti nel domino trusted possono accedere le risorse ma il contrario non e' possibile

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 16-45-29.png" alt=""><figcaption></figcaption></figure>

* **Two-way-trust**, bi-direzionale, Utenti di entrambi i domini possono accedere risorse nel dominio opposto

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 16-46-46.png" alt=""><figcaption></figcaption></figure>

### Transitivita'

* **Trust Transitivo:** Puo' essere esteso al fine di stabilire relazioni di trust con altri domini
  * Tutte le relazioni di trust intra-foresta di default (Padre-Figlio, Radice dell'albero) tra domini della stessa foresta sono definiti transitive two-way trusts
* **Trust Non-Transitivo:** Non puo' essere esteso ad altri domini nella foresta, puo' essere two-way o one-way
  * Questo e' il trust di default (chiamato external trust) tra due domini in foreste differenti dove le foreste non hanno relazioni di trust

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 16-52-24.png" alt=""><figcaption></figcaption></figure>
