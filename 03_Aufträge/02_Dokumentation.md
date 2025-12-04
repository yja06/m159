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
