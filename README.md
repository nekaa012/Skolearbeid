# Skolearbeid
Oppgave 1-•	Organisasjonen i Active Directory
Steg.1 – Rask helsesjekk av eksisterende domene-
•	PÅ DC01 kjører  jeg ipconfig as an administrator
•	Etterpå kjører nslookup dc01.ad.lab.test  - (kontrollerte at DNS kunne finne domenkontrolleren.)
•	Og fikk svar: server unknown , address – 192.168.1.1 
•	Kjører nslookup -type=SRV _ldap._tcp.dc._msdcs.ad.lab.test  - (kontrollerte DNS og  registreringer for LDAP og domenkontrolleren .) Fikk server and address . 
•	Dcdiag- kontrollerte helsetilstanden til Active Directory.   Fikk passed test .

Steg 2 – tegn OU-strukturen før du bygger. 
Før jeg begynte å opprette objekter i AD ,
I  Server Manager AD  opprett  Bedrift i domenet – først jeg opprettet hoved-OU med navnet Bedrift. Under denne opprettet jeg Brukere, Datamskiner, og Grupper.
Under Brukere opprettet jeg to OU- Administrasjon , og Support.
Høyreklikk på kubendata.local – velg Organizational Unit – Bedrift – jeg lagde i Bedrift – høyreklikk – velg Organizational Unit – Brukere
Hvis du åpner under Bedrift i AD Users and Computers, skal vi se: 
•	Brukere
•	Datamaskiner
•	Grupper
Under Brukere skal se :
•	Administrasjon 
•	Support


•	Steg 3- Opprett brukere med en navnestandard.
I AD user åpne Bedrift – Brukere- Administrasjon – new- user . fyll navn , passord . 
I OU Administrasjon 
Maria Hansen
Mona khan
Sara Nordmann
 I OU Support 
Bedrift – Brukere- support
Anna sandnes
Fillip Johansen
Lavin Wassvik

•	Kontroller med powrshell :
Get-ADUser maria.hansen -Properties DistinguishedName
Jeg fikk – CN= brukerens navn (maria.hansen).
  OU=Avdelingen (Administrasjon)
OU= Brukere 
OU= Bedrift
DC = Domenet ( DC=ad , DC=lab, DC= test)

•	Steg 4 : forskjellen mellom OU og Gruppe 

OU- orgnizational Unit 
 En OU brukes til å organisere brukere og datamskiner.
Organisere avdelinger

 Gruppe – Security group
En gruppe brukes til å samle brukere og admister tilgang.
Samle ansatte , gi tilgang til delte mapper 

Opprett globale sikkerhersgrupper 
1-	GG-Administasjon
2-	GG-support

I AD – kubendata.local – Bedrift- OU – en Grupper
OU grupper- new- Group – 
Group name -GG-Administarsjon
GROUP scope- Global
Group type- security

OPprettGG- support samme process.
Legg brukere riktig plass 
Klikk på GG-Administarsjon- properties- members-add- (skrive bruker navn ) – check names- ok – apply- ok.
Samme process med GG-support

Kontroller gruppe med powershell som administrator
Get-ADGroup GG-Administrasjon -Properties GroupScope,GroupCategory

Vise – GroupScope – Global
GroupCatagury – Sacurity

Kjøre –
Get-ADGroup GG-Support -Properties GroupScope,GroupCategory

Jeg brukte powershell komanden for å kontrollere medlemskapet i grouppen . 

Resultat- alle seks brukerne lagt til i globale sikkerhetsgrouppe .

Steg 5 – opprettelse av Domenlokale ressurgrupper 
1	– Global gruppe (GG)
2	Domenelokal gruppe (DL)
Opprettet – DL-Felles-Administrasjon-RW
        GG-Administrasjon 
                   DL-Felles-Support-RW
        GG- Support
 Bruker – GG-administrasjon- DL-Felles-Administrasjon-RW- Felles Administrasjon

 Support Brukere – GG- support- DL- felles -support-RW -  felles support

Opprett først gruppe- 
Doemn- Bedrift- Grupper – klikk på OU grupper – new- Group- fyll 
DL-Felles-Administrasjon-RW
Group scope – Domain local
Group type – Security  
Ok
 Opprett andre gruppe med samme process 

Legg globale grupper inn i domenlokale grupper –
 I DL-felles- Administrasjon- properties- member- add – skriv- GG-Administarsjon- check names – ok -apply – ok 

 Legg GG-Support inn i DL-support
Samme process med support 

Kontroller med powershell
Get-ADGroupMember DL-Felles-Administrasjon-RW
Samme process- support


Kontroller gruppetype- 
Get-ADGroup- DL-Felles-Support-RW -Properties GroupScope,GroupCategory
Begge skal ha Group scope – Domenlocal , Group category- Security . 

STEG 6. Test AD med powershell 
Get-ADUser -Filter *
Get-ADGroup -Filter *
Get-ADGroupMember GG-Support
Get-ADGroupMember DL-Felles-Support-RW
 Dette viser- Brukernavn- GG support- DL-Felles-Support-RW- Felles Support


oppgave -2 
STEG-1 -DNS før domenemdlemskap -
jeg skapt av  CLIENT01 - i Hyper-v manager , og sett op IP-address ,og DNS . 
Etterpå jeg kjørte ipconfig\all , og fikk riktig IP-address . 
Jeg kjørte nslookup.ad.lab.test , og etterpå jeg kjørte nslookup.DCO1.ad.tab.test- men 
den fungerer ikke . 
jeh sjekket server IP -address og DNS . og ping fra CLIENT01 til å se det kobblet sammen . og jeg fikk svar packet recieved .
Den jeg kjørte egen nslookup.ad.lab.test- og nslookup.DC01.ab.lab.test .
og den fungerer jeg fill Server og address name . 

 STEG 2- Meld CLIENT01 inn i domenet-
bli med CLIENT01 til domain- i PC navn - endre- domain - ad.lab.test
Men det ser her ikke kontakte AD-DC til doment ad. lab.test.
sjekket den Hyper-v switches -network adapter og fant virtual switch dropdown. velgte den Interal new virtual network switch .
gjørt samme process med CLIENT01.
kjørre IP-Address egen , og ping fra CLIENT01 til SERVER , og fikk svar.
etterpå egen jeg gi domain navn ad.lab.test- men det viser kan ikke kontakte AD-DC for domrny ad.lab.test

STEG 3- gruppebasert tilgang
jeg opprettet to mapper på SERVER 
c:\\Felles\Administrasjon
c:\\Felles\Support
Jeg konfigurerte tilgang ved hjelp av sikkerhetsgruppene GG-Administrasjon og GG-Support
Administrasjon og Support  fikk tilgang til mappen.
Jeg testet tilgangen fra CLIENT01 men en Support-bruker og en Administrasjon- bruker .


oppgave 3-
steg-1  AD-DS katalogtjenesten.
LDAP og Distinguished Name- jeg kjørte Get-ADUser -Filter * -Properties DistinguishedName|Select-Object Name, DistinguishedName
og jeg fikk bruker og identitet
CN=Mona Khan, OU=Administrasjon,OU=Brukere,DC=Kubendata,DC=local

Steg-2   hent DN med powershell
jeg brukte  powershell kommanden -  Get-ADUser og Get-ADComputer for å hente DIstinguished Name .DN viser den plasseringen til brukerobjektet 
og datamaskiobjektet i AD .

Get-ADUser ola.nordmann -Properties DistinguishedName |select-object Name,DistinguishedName
resultat er - Name- Ola Nordmann  ,
DistinguishedName :CN=Ola Nordmann,OU=OU-IT,DC=Kubendata,DC=local

Derreter kjørte denne kommand -Get-ADComputer CLIENT01 -Properties DistinguishedName  |select-object Name,DistinguishedName

sammenlign med AD-users and computers 
Åpne server manager - Tools-Active Directory Users and Computers
utvid domenet kubendata.local
finn brukeren Ola Nordmann
Finn datamskinen CLIENT01 

sammenlign plassering iADUC med OU-navnene i DN-resultatet.

STEG 3- søk i support-OU-en med SearchBase
jeg her testet med denne metoder - Get-ADUser -Filter * -SearchBase "OU=Support,OU=Brukere,OU=Bedrift,DC=ad,DC=lab,DC=test"
Men det ikke fungerer.  Derreter prøvde med denne kommanden - |
$OU=(Get-ADOrganizationalUnit -Filter 'Name -eq "Support"`).DistinguidhedName
og jeg fikk resultatet. det står brukerene som ligger i support-OU-en og underliggende OU-er. 

søk med LDAP -Filter
Get-ADUser -LDAPFilter "(sAMAccountName=ola.nordmann)"
og fått informasjon om brukeren Ola.nordmannn.
 jeg har testet forskjellege søkemetoder i AC .Med serachbase kan jeg søke etter brukere
 i en bestemt OU, for eksemple support .Med LDAP filter søke etter en bestemt bruker ved hjlep av sAMSccountName.

 Steg4- Koble LDAP til nettverksuka 
fra CLIENT01 - i pwershell kommand - jeg kjørte denne kommand 
Test-NetConnection dc01.ad.lab.test -Port 389
men vi fikk Pingsucceeded: False 

jeg tester forbindelse fra CLIENT01 til serevr . DNS finner IP addressen til domenkontrolleren 
TCP-oppretter forbindelsen ,og  LDAP brukes til å kommunisere med AD. DEtte viser sammenhengen mellom DNS, IP-address

Oppgave - 4 
steg-1- en GPO faktisk gjør
Først finn brukeren in Domain -kubendata.local
 Bruker ligger i OU-Bedrift-brukere-support
 Datamaskiner ligger i OU-Bedrift
 Innstillingen ligger under user configuration 
fikk GPO-Administrashon-Brukerinnstilling  er som  koblet til OU- Administrasjon . sammen med support .

steg-2   lag en trygg brukerpolicy
Jeg opprettet en ny GPO med navnet GPO-support-Brukerinnstilling i Group Policy Management.
I OU- en Support under velget create a GPO in this domain, and link here. 
skrivet navnet - GPO-Support-Brukerinstilling. 
Etter i GPO-Support-Brukerinstilling jeg velget edit -user configuration-policies-Administrative Templates -control panel. 
Jeg koblet GPO-en til Support-OU-en i Active Directory.Deretter valgte jeg en enkel brukerinnstilling under user configuration.

Steg 3- oppdater og bevis at policyen gjelder
Jeg testet GPO-en på CLIENT01 i poershell med support-bruker . 
Jeg brukte gpupdate/force for å oppdatere policyen og 
gpresult/r for å kontrollere om GPO- en ble brukt.
Jeg kontrollerte at GPO-Support-Brukerinnstilling vises under Applied Group Policy Obkects , dette viser at GPO-en gjelder for support-brukeren.


STEG4 - AD -detektiv
1. Symptom
Support-brukeren fikk ikke tilgang til Support-mappen.
2. Hypotese
Jeg trodde at GG-Support manglet medlemskap i DL-Felles-Support-RW.
3. Test
Jeg åpnet PowerShell på DC01 og kjørte:
Get-ADGroupMember DL-Felles-Support-RW
4. Funn
Jeg fant ut at GG-Support ikke var medlem av DL-Felles-Support-RW.
5. Løsning
Jeg la GG-Support til i tilgangsgruppen:
Add-ADGroupMember -Identity DL-Felles-Support-RW -Members GG-Support
Deretter kontrollerte jeg medlemskapet på nytt.

Etter at jeg rettet gruppeinnstillingene, testet jeg tilgangen til Support-mappen på CLIENT01.











