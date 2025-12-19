# Domain Enumeration - Trusts2

### **Default/Automatic Trusts**

* Trust Padre/Figlio
  * E' creato automaticamente tra il nuovo dominio e il dominio che lo precede nella gerarchia del namespace, ogniqualvolta un nuovo dominio venga aggiunto. (Es. xxx.yyy.com e' figlio di yyy.com)
  * Questo tipo di trust e' sempre two-way transitive
* Trust della radice dell'albero
  * E' creato automaticamente quando un nuovo albero dei domini (domain tree) e' aggiunto alla radice della foresta (forest roo)
  * Questo tipo di trust e' sempre two-way transitive

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 16-58-53.png" alt=""><figcaption></figcaption></figure>

### **External Trusts**

* Tra due domini in foreste differenti quando le foreste non hanno relazioni di trust
* Puo' essere sia one-way che two-way transitive

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 17-00-41.png" alt=""><figcaption></figcaption></figure>

### **Forest Trusts**

* Tra forest root domain
* Non puo' essere estesa ad una terza foresta (no implicit trust)
* Puo' esserer sia one-way che two-way transitive

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
