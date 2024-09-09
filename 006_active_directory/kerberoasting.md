# Kerberoasting

When we send TGS-REQ to a KDC, it does not check if we have access to the service and provide us a TGS encrypted with the service account password hash.

A service account is an account that has at least one SPN attribute set.

**Kerberoasting technique = getting TGS from all accounts with SPN and crack it offline the recover password**

a
