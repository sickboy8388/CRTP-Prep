# Persistence using ACLs - Security Descriptors

* E' possibile modificare i Security Descriptor ( informazioni di sicuezza come, Owner, primary group, DACL e SACL) di diversi metodi di accesso remoto (securable objects) per garantire l'accesso ai non-admin users
* Sono richiesti privilegi amministrativi per questo&#x20;
* Il Security Descriptor Definition Language (SDDL) definisce il formato che e' usato per descrivere un Security Descriptor, SDDL utilizza stringhe ACE per DACL e SACL
* ACE per Administrator built-in per namespace WMI

**A;CI;CCDCLCSWRPWPRCWD;;;SID**
