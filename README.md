## 🚀 Windows Server-oppsett med PowerShell
Dette prosjektet automatiserer oppsettet av en Windows Server med Active Directory, DNS, DHCP, filserver, Hyper-V og VLAN ved hjelp av PowerShell. 

Følg stegene nedenfor for å konfigurere serveren din effektivt.


## Steg-for-steg veiledning
## 1️⃣ Installer nødvendige roller og funksjoner
Kjør følgende kommandoer i PowerShell som administrator for å installere nødvendige tjenester:
```powershell

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-WindowsFeature -Name DNS -IncludeManagementTools
Install-WindowsFeature -Name DHCP -IncludeManagementTools
Install-WindowsFeature -Name FS-FileServer -IncludeManagementTools
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
Install-WindowsFeature Web-Server
```
✅ Sjekk installasjonen i Server Manager → Dashboard. Hvis noen funksjoner mangler, prøv på nytt eller start serveren på nytt.



## 2️⃣ Sett en statisk IP-adresse
Konfigurer en statisk IP for serveren:
```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.10 -PrefixLength 24 -DefaultGateway 192.168.1.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.1.1
```
🔹 Husk: Kjør alltid PowerShell som administrator.


## 3️⃣ Endre navn på serveren
Endre servernavnet og start på nytt:
```powershell
Rename-Computer -NewName "Server01" -Force -Restart
```
✅ Sjekk det nye navnet i Server Manager → Local Server.


## 4️⃣ Gjør serveren til en domenekontroller
```powershell
Install-ADDSForest -DomainName "dittdomene.local"
```
🔹 Bytt ut "dittdomene.local" med ønsket domene (f.eks. kuben.local).
🔹 Lag et sterkt passord, bekreft det, skriv "A" og trykk Enter.


## 5️⃣ Start serveren på nytt
Etter installasjonen av Active Directory må serveren startes på nytt:
```powershell
Etter installasjonen av Active Directory må serveren startes på nytt:
Restart-Computer -Force
```

## 6️⃣ Opprett en Organisatorisk Enhet (OU)
Lag en OU (f.eks. IT-Avdeling):
```powershell
New-ADOrganizationalUnit -Name "IT-Avdeling" -Path "DC=kuben,DC=local"
```


## 7️⃣ Sjekk at OU-en er opprettet
List opp alle OUs for å bekrefte:
```powershell
Get-ADOrganizationalUnit -Filter * | Select Name, DistinguishedName

```

8️⃣ Endre standard OU for nye brukere
Flytt nye brukere automatisk til IT-Avdeling i stedet for standard Users-mappen:
```powershell
Redircmp "OU=IT-Avdeling,DC=kuben,DC=local"

```

9️⃣ Opprett en CSV-fil for brukere

1. For å opprette flere brukere samtidig, må du først lage en CSV-fil i Notepad:

2. Åpne Notepad

3. Skriv inn følgende (tilpass etter behov):
```powershell
FirstName,LastName,Username,Group
Jonas,Yalkhoroev,jyk,Student
Maria,Nilsen,mni,Teacher
Ola,Normann,on,Student
```

3. Lagre filen som CSV:

. Klikk File → Save As

. Velg "All Files"

. Gi den navnet ClassWorks.csv

. Pass på at filen ender med .csv

. Klikk Save

✅ CSV-filen er nå klar til å importere brukere i Active Directory!
