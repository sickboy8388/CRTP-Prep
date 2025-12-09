# LEARNING OBJECTIVE16

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

* Enumerate users in the domain for whom Constrained Delegation is enabled.
  * For such a user, request a TGT from the DC and obtain a TGS for the service to which delegation is configured.
  * &#x20;Pass the ticket and access the service as DA.
* &#x20;Enumerate computer accounts in the domain for which Constrained Delegation is enabled.
  * For such a user, request a TGT from the DC.
  * Use the TGS for executing the DCSync attack.
