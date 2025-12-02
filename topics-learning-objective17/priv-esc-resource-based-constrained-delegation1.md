# Priv Esc - Resource-based Constrained Delegation1

* Qui  l'autorità di delega delle risorse/servizi e' trasferito all'amministratore.
* Invece che dagli SPN su msDs-AllowedToDelegateTo come per i webservices, qui l'accesso e' regolamentato dal Security Descriptor di di msDS-AllowedToActOnBehalfOfOtherIdentity (visibile come PrincipalsAllowedToDelegateToAccount) sulla risorsa/servizio come\
  il servizio SQL Server.
* Ciò significa che l'amministratore delle risorse/dei servizi può configurare questa delega,\
  mentre per altri tipi sono necessari i privilegi SeEnableDelegation,\
  che, per impostazione predefinita, sono disponibili solo per gli amministratori di dominio.
