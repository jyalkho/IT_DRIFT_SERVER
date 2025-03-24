🚀 Windows Server-oppsett med PowerShell
Dette prosjektet automatiserer oppsettet av en Windows Server med Active Directory, DNS, DHCP, filserver, Hyper-V og VLAN ved hjelp av PowerShell. Følg stegene nedenfor for å konfigurere serveren din effektivt.


📌 Steg-for-steg veiledning
1️⃣ Installer nødvendige roller og funksjoner
Kjør følgende kommandoer i PowerShell som administrator for å installere nødvendige tjenester:

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-WindowsFeature -Name DNS -IncludeManagementTools
Install-WindowsFeature -Name DHCP -IncludeManagementTools
Install-WindowsFeature -Name FS-FileServer -IncludeManagementTools
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
Install-WindowsFeature Web-Server

✅ Sjekk installasjonen i Server Manager → Dashboard. Hvis noen funksjoner mangler, prøv på nytt eller start serveren på nytt.
