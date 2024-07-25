# Kerberoasting
Kerberos uses Service Principal Names (SPNs) to link service instances to logon accounts, allowing client applications to request authentication without knowing the account name. Kerberoasting exploits this by obtaining a Ticket Granting Service (TGS) ticket, which is encrypted with the service account's NTLM hash, and then performing offline password cracking.
