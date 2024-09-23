# Golden Ticket

When a user asks the KDC for a TGT, the TGT is encrypted by a secret key.

The secret key is the password hash of a domain user account which is krbtgt.

If we can get our hands on the _krbtgt_ password hash, we could create our own self-made custom TGTs, also known as **Golden Tickets**

Good mean of persistence : The best advantage is that the _krbtgt_ account password is not automatically changed.

{% hint style="info" %}
krbtgt password hash has to be securely stored when it is stolen during an operation because it grants unlimited domain access.
{% endhint %}
