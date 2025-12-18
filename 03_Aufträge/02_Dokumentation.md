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

<img width="2810" height="1198" alt="ping erlauben dc" src="https://github.com/user-attachments/assets/076c3760-7c7c-47c9-a975-99d6bf183970" />

### 2.2 DNS-Server konfigurieren

* DNS-Server auf DC-IP setzen:

### 2.3 Active Directory Domain Services installieren

* Server Manager öffnen
* Add Roles and Features
* Active Directory Domain Services auswählen
* Installation durchführen
* 
<img width="3618" height="1964" alt="AD heruntergeldaen auf server" src="https://github.com/user-attachments/assets/f074ebac-5af3-4b4d-8d27-09ffa45d4047" />

### 2.4 Domain Controller promovieren

* Nach ADDS-Installation: "Promote this server to a domain controller"
* Add a new forest
* Root domain name: `ec2.yenul.m159`
* DSRM-Passwort festlegen
* DNS-Server wird automatisch installiert
* NetBIOS-Name: `EC2`
* Installation abschließen und Neustart

* **Screenshot:**

<img width="3612" height="1988" alt="DC Server mit Script konfigurieren" src="https://github.com/user-attachments/assets/472bb2a9-750b-46f2-9f42-d5e93e74bf9b" />

Zu controller promoten

<img width="3640" height="1960" alt="promote server to dc" src="https://github.com/user-attachments/assets/737c31d2-8d3b-4aba-8cc0-098a7929ffee" />

### Instanz Backupen zur Sicherheit

<img width="3370" height="1570" alt="Backup DC Instanz" src="https://github.com/user-attachments/assets/c1622dfd-f533-4a07-ae95-dcc10b59ec64" />


### 2.5 DNS-Zonen überprüfen

* DNS Manager öffnen
* Forward Lookup Zones prüfen
* Reverse Lookup Zones prüfen

* **Screenshots:**

<img width="2126" height="1260" alt="nslookup nach dc erstellung" src="https://github.com/user-attachments/assets/8a1afa86-1c4f-4511-8dae-fab97a99d4bb" />


<img width="3628" height="1970" alt="nslookup srv nach dc erstellung" src="https://github.com/user-attachments/assets/82223787-85f9-4b11-b6b3-b4dcfbe4890c" />

<img width="1528" height="1030" alt="neue dns reverse lookup zone" src="https://github.com/user-attachments/assets/7e4a1e0a-ce27-41f7-96d6-15b20bbfd0d3" />


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


* **Screenshot:**

<img width="2610" height="1674" alt="dns server auf client vm auf dc zuweisen" src="https://github.com/user-attachments/assets/39e1d69d-6568-4d29-af48-6fe87d41cd0e" />


### 3.2 DNS-Konfiguration testen

<img width="2602" height="1688" alt="nslookup auf client vm" src="https://github.com/user-attachments/assets/e12ee480-0785-4ce9-9160-0e4c817887b4" />


### 3.3 Client zur Domain joinen


* Benutzername: `ec2\Administrator`
* Passwort: Tbz12344$

* **Screenshots:**

<img width="2600" height="1702" alt="Domain Join" src="https://github.com/user-attachments/assets/5f1b5035-dcc1-4000-a62b-4414f388b683" />

<img width="2550" height="1666" alt="Domain join success" src="https://github.com/user-attachments/assets/6cff6cce-2fa5-4d83-b323-aafe6209436f" />

### 3.4 Mit Domain-Admin anmelden

Nach dem Neustart:
* Bei der Anmeldung: "Anderer Benutzer"
* Benutzername: `ec2\Administrator`
* Passwort: Tbz12344$

### 3.5 DNS-Dienst auf DC konfigurieren

**Problem:** DC zeigte "localhost" bei nslookup statt eigenen DNS-Server

<img width="2146" height="1658" alt="nslookup auf dc zu client" src="https://github.com/user-attachments/assets/b50228bb-2256-422f-a4a3-ed3702afecee" />

