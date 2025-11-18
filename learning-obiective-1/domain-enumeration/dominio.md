# Dominio

#### Ottenere informazioni rispetto il dominio attuale&#x20;

<kbd>**`Get-Domain (`**</kbd>`PowerView)`

&#x20;`Get-ADDomain` (ActiveDirectory Module)

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**&#x20;

**`Get-Domain -Domain`**` ``moneycorp.local`\
`Get-ADDomain -Identity moneycorp.local`\


#### Otttenere il SID di dominio per l'attuale dominio

**`Get-DomainSID`**\
`(Get-ADDomain).DomainSID`
