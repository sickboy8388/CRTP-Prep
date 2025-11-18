# User

#### Ottenere lista utenti dominio attuale

<kbd>**`Get-DomainUser (`**</kbd>`PowerView)`

<kbd>**`Get-DomainUser -Identity`**</kbd> student1<kbd>`(PowerView)`</kbd>

&#x20;`Get-ADUser -Filter * -Properties *` (ActiveDirectory Module)

`Get-ADUser -Identity` student1 `-Properties *` (ActiveDirectory Module)

#### Ottenere lista di tutte le proprieta' degli utenti nel dominio attuale

<kbd>**`Get-DomainUser`**</kbd><kbd>` `</kbd><kbd>`-Identity`</kbd> student1 <kbd>`-Properties`</kbd> \* <kbd>`(PowerView)`</kbd>&#x20;

&#x20;**`Get-DomainUser`** `-Properties` samaccountname,logonCount  <kbd>`(PowerView)`</kbd>

`Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member - MemberType *Property | select Name` (ActiveDirectory Module)

`Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}}` (ActiveDirectory Module)

#### Ricerca di una stringa arbitraria negli user attributes

<kbd>**`Get-DomainUser`**</kbd>

