# Initial Setup – AWS Infrastruktur & Windows Basis-Konfiguration

## 1. AWS Infrastruktur erstellen

### 1.1 AWS Managed AD Security Group konfigurieren
* Sicherheitsregeln für Domain-Services (LDAP, Kerberos, DNS, RPC, SMB, etc.)
* **Screenshot:**  
<img width="2096" height="1642" alt="AWS Managed AD Security Group" src="https://github.com/user-attachments/assets/fd7eec02-46b9-4c7c-bdcf-fa8d26cd5104" />


### 1.2 Client Security Group erstellen
* RDP, DNS, SMB, Kerberos etc. für Client erlauben
* **Screenshot:**  
<img width="3766" height="1674" alt="ClientSecurityGrouperstellen" src="https://github.com/user-attachments/assets/cd958873-efa0-4294-b4be-680d9cab75b2" />


### 1.3 Elastic IPs erstellen und Instanzen zuweisen
* Bei EC2 → Elastic IPs generieren und an DC, Client, Admin Center binden
* **Screenshot:**
<img width="3836" height="1144" alt="Elastic IPS erstellt und zugewiesen" src="https://github.com/user-attachments/assets/4d4e8a99-6d31-477d-9394-4b3d6b4d969d" />


### 1.4 EC2 Instanzen erstellen
* Windows Server 2022 (DC, Client, Admin Center) im jeweils passenden Subnetz
* Privat-/Public-IP festlegen, Security Group auswählen, Key Pair speichern
* **Screenshot:**  
<img width="3840" height="1212" alt="Erstellung Instanzen" src="https://github.com/user-attachments/assets/5e5966c6-e7cb-48f9-bf4e-4fc4b4ec5157" />


### 1.5 Inbound Rules überprüfen
* Prüfen, ob alle Ports in Security Group wie geplant offen sind
* **Screenshot:**  
<img width="1872" height="1696" alt="Inbound Rules" src="https://github.com/user-attachments/assets/08a64e80-56a3-4955-97a2-44c6c9babb6e" />


### 1.6 Internetgateway erstellen
* VPC → Internet Gateways → neues GW erstellen
* **Screenshot:**  
<img width="3822" height="1490" alt="Internetgatewayerstellen" src="https://github.com/user-attachments/assets/9c5eba63-2655-4909-ab78-abec5b605bdf" />


### 1.7 Internetgateway mit VPC verknüpfen
* Internet Gateway mit deiner VPC verbinden
* **Screenshot:**  
<img width="3826" height="1686" alt="Internetgatewaymitvpcverknuepfen" src="https://github.com/user-attachments/assets/03ac9752-e2e9-4d46-a76f-6509725b2f3d" />


### 1.8 Subnetze anlegen (public/private)
* 2x Public: z.B. für DC und Client/Admin Center  
* 2x Private: z.B. für Managed AD und Backup
* **Screenshots:**  
<img width="3744" height="1612" alt="privateSubnetzerstellen1" src="https://github.com/user-attachments/assets/903ed034-b713-4f9c-b2a9-384d19e2444b" />
<img width="3772" height="1732" alt="privateSubnetzerstellen2" src="https://github.com/user-attachments/assets/85446fef-9f24-479b-8ff5-2789c6ef39fa" />
<img width="3822" height="1116" alt="Routehinzufuegen" src="https://github.com/user-attachments/assets/17ecc7fe-72a9-4386-87a9-0f678cf70639" />
<img width="3800" height="1684" alt="Subnetz2erstellen" src="https://github.com/user-attachments/assets/81922105-79c3-4b63-bd64-6d4b36bbd5c0" />


### 1.9 Routing Table erstellen und Subnetze zuordnen
* Public Subnet Routing Table mit Gateway verknüpfen
* **Screenshots:**  
<img width="3826" height="1536" alt="RoutingTabelleerstellen" src="https://github.com/user-attachments/assets/fe9f0e94-1dcd-49a4-8057-3db6a8fa95a8" />
<img width="3822" height="1116" alt="Routehinzufuegen" src="https://github.com/user-attachments/assets/fafa3f8b-6a8a-4058-a2df-d31c492d3f61" />
<img width="3814" height="1450" alt="SubnetzRouteTablehinzufuegen" src="https://github.com/user-attachments/assets/d5f6c394-5394-4dd4-9553-22effb649e86" />


---

## 2. Nach dem Start der DC-Instanz: Windows Grundkonfiguration

### 2.1 Ping auf DC erlauben
* PowerShell als Administrator:  


---

## 3. Client zur Domain joinen

### 3.1 Netzwerkkonfiguration prüfen

**Problem identifiziert:**
* Client und DC befanden sich in verschiedenen Subnetzen:
  * Client: `10.0.2.20` (Subnetz 10.0.2.0/24)
  * DC: `10.0.1.10` (Subnetz 10.0.1.0/24)
  * DNS-Server auf Client zeigte auf falsche IP: `10.0.0.10` (existiert nicht)

**Lösung:**
* DNS-Server auf dem Client korrigieren:

```powershell
# DNS auf die richtige DC-IP setzen
Set-DnsClientServerAddress -InterfaceAlias "Ethernet 3" -ServerAddresses "10.0.1.10"

# Überprüfen
Get-DnsClientServerAddress -InterfaceAlias "Ethernet 3"

# DNS Cache leeren
ipconfig /flushdns
```

### 3.2 DNS-Konfiguration testen

```cmd
# DNS-Auflösung testen
nslookup ec2.yenul.m159

# Ping zum DC
ping ec2.yenul.m159
ping 10.0.1.10
```

### 3.3 Client zur Domain joinen

**PowerShell-Methode:**
```powershell
Add-Computer -DomainName "ec2.yenul.m159" -Credential (Get-Credential) -Restart
```
* Benutzername: `ec2\Administrator`
* Passwort eingeben
* System startet automatisch neu

**GUI-Methode:**
1. Windows-Taste + Pause → Systemeinstellungen
2. "Einstellungen ändern" (bei Computername)
3. Button "Ändern..."
4. Wähle: "Domäne:"
5. Eingabe: `ec2.yenul.m159`
6. OK klicken
7. Domain-Admin Credentials eingeben:
   * Benutzername: `Administrator` oder `ec2\Administrator`
   * Passwort: [DC Admin-Passwort]
8. Willkommensnachricht erscheint ✅
9. Neustart durchführen

### 3.4 Mit Domain-Admin anmelden

Nach dem Neustart:
* Bei der Anmeldung: "Anderer Benutzer"
* Benutzername: `ec2\Administrator` oder `Administrator@ec2.yenul.m159`
* Passwort eingeben

**Überprüfung:**
```cmd
whoami
# Sollte anzeigen: ec2\administrator

echo %USERDOMAIN%
# Sollte anzeigen: EC2
```

```powershell
# Domänenmitgliedschaft prüfen
Get-ComputerInfo | Select-Object CsDomain, CsDomainRole

# Gruppenrichtlinien aktualisieren
gpupdate /force
```

### 3.5 DNS-Dienst auf DC konfigurieren

**Problem:** DC zeigte "localhost" bei nslookup statt eigenen DNS-Server

**Lösung auf dem DC:**
```powershell
# DNS auf sich selbst setzen
Set-DnsClientServerAddress -InterfaceAlias "Ethernet 3" -ServerAddresses "10.0.1.10","127.0.0.1"

# DNS-Cache leeren und neu registrieren
ipconfig /flushdns
ipconfig /registerdns

# DNS-Dienst neu starten
Restart-Service DNS

# Testen
nslookup YJA-Client
nslookup ec2.yenul.m159
```

**DNS-Zonen prüfen:**
```powershell
# Forward Lookup Zone prüfen
Get-DnsServerZone

# A-Records anzeigen
Get-DnsServerResourceRecord -ZoneName "ec2.yenul.m159" -RRType A
```

---

**Status:** ✅ Client erfolgreich zur Domain `ec2.yenul.m159` gejoint
