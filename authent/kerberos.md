# Kerberos

La différence majeure avec NTLM est que le client n'interagit pas avec le serveur pour s'authentifier mais avec le KDC (Key Distribution Center).

Le rôle de KDC est souvent porté par le Domain Controller.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## Step 1 = AS-REQ

Le timestamp est chiffré via le hash du mdp de l'utilisateur

## Step 2 = AS-REP

Si le hash correspond à celui de l'utilisateur dans la base ntds.dit alors OK

Contenu de l'AS-REP :&#x20;

* TGT chiffré avec le NTLM du mdp du compte krbtgt
* Session key chiffrée avec le hash du mdp de l'utilisateur

Step 3 =&#x20;

