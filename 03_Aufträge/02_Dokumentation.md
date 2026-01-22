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

### 3.6 Remote Desktop Konzept - RDP-Zugriff für Benutzer

---

## Umsetzung

### 1. AD-Gruppen erstellen

**Auf dem Domain Controller:**

1. **Active Directory Users and Computers** öffnen
   - Start → "Active Directory Users and Computers" eingeben


2. **Erste Gruppe erstellen (RDP-Admins)**
   - Rechtsklick auf Container **Users** → **New** → **Group**
   - Group name: `RDP-Admins`
   - Group scope: `Global`
   - Group type: `Security`
   - **OK** klicken



3. **Zweite Gruppe erstellen (RDP-Users)**
   - Rechtsklick auf Container **Users** → **New** → **Group**
   - Group name: `RDP-Users`
   - Group scope: `Global`
   - Group type: `Security`
   - **OK** klicken
<img width="3644" height="1964" alt="User für Rdp verbindung erstellen" src="https://github.com/user-attachments/assets/7048137c-0607-427a-87bd-7748ca6c3cb3" />
<img width="2818" height="2114" alt="user gruppe" src="https://github.com/user-attachments/assets/c45897c4-7341-4b66-92d5-cf6457f131e3" />


---

### 2. Testbenutzer erstellen

1. **Neuen Benutzer anlegen**
   - Rechtsklick auf **Users** → **New** → **User**
   - First name: `Test`
   - Last name: `User`
   - User logon name: `testuser`
   - **Next** klicken

<img width="2134" height="1738" alt="test rdp user" src="https://github.com/user-attachments/assets/14a1847c-f125-40b8-969e-e964d1efd931" />


2. **Passwort setzen**
   - Password: `Tbz12344`
   - Confirm password: `Tbz12344`
 

---

### 3. Benutzer zu Gruppen hinzufügen

1. **Administrator zur RDP-Admins Gruppe hinzufügen**
   - Doppelklick auf Gruppe **RDP-Admins**
   - Tab **Members** → **Add...**
   - `Administrator` eingeben
   - **Check Names** → **OK** → **OK**

<img width="1762" height="1318" alt="Admin zu Rdp Admin hinzufügen" src="https://github.com/user-attachments/assets/8b80cef3-8146-472d-ac20-51a7bd505742" />


2. **TestUser zur RDP-Users Gruppe hinzufügen**
   - Doppelklick auf Gruppe **RDP-Users**
   - Tab **Members** → **Add...**
   - `testuser` eingeben
   - **Check Names** → **OK** → **OK**



---

### 4. RDP-Berechtigungen auf dem DC zuweisen

1. **Local Security Policy öffnen**
   - Win+R → `sysdm.cpl` → Enter
<img width="2100" height="1394" alt="add user to sysdm cpl" src="https://github.com/user-attachments/assets/c16ffb92-918f-4433-9231-e6e3135b15fd" />
<img width="1728" height="1224" alt="user added to sysdm cpl" src="https://github.com/user-attachments/assets/262cd3ed-71ef-485f-bf0a-1722d229c648" />
<img width="2026" height="1676" alt="add user to sysdm cpl p 2" src="https://github.com/user-attachments/assets/8689890c-0ca7-437a-a7ad-22f2ac3d9a41" />

 

### 4. RDP-Berechtigungen auf dem DC zuweisen

1. **Local Security Policy öffnen**
   - Win+R → `secpol.msc` → Enter

2. **User Rights Assignment öffnen**
   - **Local Policies** erweitern → **User Rights Assignment** klicken

<img width="2070" height="1448" alt="rpd users berechtigung geben fürs verbinden" src="https://github.com/user-attachments/assets/d2a19e5f-9452-4b5c-9b55-cb2fb1354bb1" />


3. **RDP-Rechte zuweisen**
   - Doppelklick auf **"Allow log on through Remote Desktop Services"**
   - **Add User or Group...** klicken
   - `EC2\RDP-Users` eingeben → **Check Names** → **OK**
   - **Add User or Group...** klicken
   - `EC2\RDP-Admins` eingeben → **Check Names** → **OK**
   - **OK** klicken

<img width="2994" height="1640" alt="allow rdp verbindung" src="https://github.com/user-attachments/assets/7b1d056a-8646-422f-93b0-9d755429286b" />


---

### 5. RDP-Verbindung testen

**Vom Client zum DC verbinden:**

1. **Remote Desktop Connection öffnen**
   - Win+R → `mstsc` → Enter
   - Computer: `10.0.1.10` (DC Private IP)
   - **Connect** klicken

<img width="3634" height="1984" alt="test von client auf dc kommen" src="https://github.com/user-attachments/assets/7dc52fbe-dc59-42dc-b5da-9a181b010132" />


2. **Als testuser anmelden**
   - Username: `EC2\testuser`
   - Password: `Tbz12344`
   - **OK** klicken



3. **Erfolgreiche Verbindung**
   - Desktop des Domain Controllers wird angezeigt
   - RDP-Zugriff funktioniert für normale Benutzer

<img width="3654" height="1976" alt="rdp funktioniert" src="https://github.com/user-attachments/assets/30131a5e-de44-4c2d-9fda-52d04ea0f998" />
<img width="3638" height="1954" alt="rdp beweis" src="https://github.com/user-attachments/assets/f8c52ad9-137c-4000-91cc-e1d8ee261df0" />


---

## Aufgabe 4: Freigaben, Laufwerke & Berechtigungen

### Phase 1: Benutzer und Gruppen anlegen

#### 1. Abteilungsgruppen erstellt

Im Active Directory wurden folgende Abteilungsgruppen erstellt:

- **Sekretariat** (Global Security Group)
- **Buchhaltung** (Global Security Group)
- **GL** (Global Security Group)
- **Promoter** (Global Security Group)

**Vorgehensweise:**
1. Active Directory Users and Computers öffnen
2. Rechtsklick auf Container "Users" → New → Group
3. Gruppenname eingeben, Group scope: Global, Group type: Security
   
<img width="1976" height="1344" alt="Sekretariat gruppe erstellen" src="https://github.com/user-attachments/assets/76d6157d-8c5e-45a4-a8ec-0612e2ce59d9" />

<img width="2958" height="1836" alt="Buchhaltung gruppe erstellen" src="https://github.com/user-attachments/assets/d551fa7b-5c62-4193-a303-7680464a2089" />

<img width="2336" height="1582" alt="GL gruppe erstellen" src="https://github.com/user-attachments/assets/ba39546f-25c2-47b9-905a-61a2161ab7db" />

<img width="3110" height="1830" alt="promoter gruppe erstellen" src="https://github.com/user-attachments/assets/aef5bd18-8551-4e4e-8eda-b671da7e8667" />

---

#### 2. Globale Gruppen "intern" und "extern" erstellt

Zwei übergeordnete Gruppen zur Strukturierung:

- **intern** (Global Security Group) - für interne Abteilungen
- **extern** (Global Security Group) - für externe Abteilungen

<img width="2428" height="1592" alt="intern gruppe erstellen" src="https://github.com/user-attachments/assets/8934b9a6-617c-4940-bbb5-21d13ed92dd0" />

<img width="2318" height="1336" alt="extern gruppe erstellen" src="https://github.com/user-attachments/assets/85071cef-cf94-40a6-bcfb-8a8c6175dd94" />

---

#### 3. Benutzer angelegt

Pro Abteilung wurde ein Testbenutzer erstellt:

| Abteilung | Benutzername | Vorname | Nachname | Passwort |
|-----------|--------------|---------|----------|----------|
| Sekretariat | s.mueller | Sandra | Mueller | Tbz12344$ |
| Buchhaltung | m.schmidt | Marco | Schmidt | Tbz12344$ |
| GL | a.weber | Anna | Weber | Tbz12344$ |
| Promoter | t.klein | Thomas | Klein | Tbz12344$ |

<img width="2794" height="1832" alt="User erstellen" src="https://github.com/user-attachments/assets/5e0a5d86-649d-46d4-88aa-b731c82bc7fe" />

<img width="2108" height="1352" alt="User erstellen 2" src="https://github.com/user-attachments/assets/934b18a2-5da3-42c4-ac33-723b7ef1b95d" />

<img width="2536" height="1634" alt="User erstellen 3" src="https://github.com/user-attachments/assets/f757c098-0261-445e-ac63-caddee2d285b" />

<img width="2590" height="1742" alt="User erstellen 4" src="https://github.com/user-attachments/assets/6f258e1c-ac49-4592-89e1-1e7824aa8539" />

---

#### 4. Benutzer zu Abteilungsgruppen zugeordnet

Jeder Benutzer wurde seiner entsprechenden Abteilungsgruppe hinzugefügt:

- s.mueller → Sekretariat
- m.schmidt → Buchhaltung
- a.weber → GL
- t.klein → Promoter

**Vorgehensweise:**
1. Doppelklick auf Abteilungsgruppe
2. Tab "Members" → Add
3. Benutzername eingeben → Check Names → OK


<img width="2042" height="1170" alt="user zu gruppe hinzufügen" src="https://github.com/user-attachments/assets/78cbb5c7-683a-429d-b59f-52f5454b1431" />

<img width="2070" height="1628" alt="user zu gruppe hinzufügen 2" src="https://github.com/user-attachments/assets/289942ab-8041-438d-95d9-77a3a13f3165" />

<img width="2362" height="1502" alt="user zu gruppe hinzufügen 3" src="https://github.com/user-attachments/assets/598f28b9-4bce-4178-99b3-79cec78b0060" />

<img width="1852" height="1076" alt="user zu gruppe hinzufügen 4" src="https://github.com/user-attachments/assets/2a038316-00c1-4dce-9423-f32d968bb17a" />

---

#### 5. Abteilungsgruppen zu intern/extern zugeordnet

**Gruppe "intern" enthält:**
- Sekretariat
- Buchhaltung
- GL

<img width="2000" height="1486" alt="gruppe zu gruppe intern hinzufügen" src="https://github.com/user-attachments/assets/c903f524-3ce9-43d4-b612-e8a10dca56d8" />

**Gruppe "extern" enthält:**
- Promoter


<img width="2248" height="1442" alt="gruppe zu gruppe extern hinzufügen" src="https://github.com/user-attachments/assets/82f11dd4-171f-4707-a022-7fdacedf94ef" />


---

### Phase 2: Ordnerstruktur und Freigaben erstellen

#### 1. Hauptordner für Freigaben erstellt

Auf dem Domain Controller wurde der zentrale Freigabe-Ordner erstellt:
- Pfad: `C:\Shares`

<img width="3292" height="1850" alt="Schares folder erstellen" src="https://github.com/user-attachments/assets/3db52ae3-ebac-4bce-9161-24feb872dffc" />


---

#### 2. Ordnerstruktur erstellt

Folgende Struktur wurde unter `C:\Shares` angelegt:

C:\Shares
├── Daten
│ ├── Buchhaltung
│ ├── GL
│ ├── Sekretariat
│ └── Pool
└── Aussendienst


<img width="2206" height="1342" alt="Shares unterordner" src="https://github.com/user-attachments/assets/2fe1d90d-9cd3-46c1-8df7-6507e556d250" />

<img width="2260" height="1260" alt="Shares unterordner 2" src="https://github.com/user-attachments/assets/3c72d7f7-9984-4413-9651-7a8a7bdcd7b2" />

<img width="1626" height="1078" alt="Daten Ordner sharen" src="https://github.com/user-attachments/assets/b06cddd7-872c-4afe-b43b-af13cd6d2e99" />

---

#### 3. Freigaben erstellt

**Freigabe "Daten":**
- Freigabename: `Daten`
- UNC-Pfad: `\\dc1\Daten`
- Freigabeberechtigung: Everyone = Change

<img width="928" height="1172" alt="Permission Daten" src="https://github.com/user-attachments/assets/162e9ce7-b361-476f-9095-a6822832b04c" />


**Freigabe "Aussendienst":**
- Freigabename: `Aussendienst`
- UNC-Pfad: `\\dc1\Aussendienst`
- Freigabeberechtigung: Everyone = Change

<img width="1580" height="1128" alt="Aussendienst sharing" src="https://github.com/user-attachments/assets/605b31c1-fe91-4c69-bc5a-4f70a6ba0405" />

<img width="1016" height="1202" alt="Permission Aussendienst" src="https://github.com/user-attachments/assets/f570dcc0-f0fc-42b2-8989-df0ed1d56de2" />


---

#### 4. Vererbung deaktiviert

Für den Ordner `Daten` und alle Unterordner wurde die Vererbung deaktiviert:
- Methode: "Convert inherited permissions into explicit permissions"
- Betroffen: Daten, Buchhaltung, GL, Sekretariat, Pool

**Vorgehensweise:**
1. Ordner → Properties → Security → Advanced
2. "Disable inheritance" → Convert

<img width="1698" height="1134" alt="Veerbung deaktivieren" src="https://github.com/user-attachments/assets/51f80d07-72c4-47e2-ab2b-d630d4f58ca0" />


---

#### 5. Domänenbenutzer-Gruppe entfernt

Die Standardgruppe "Domain Users" wurde von allen Ordnern entfernt, um eine saubere Berechtigungsstruktur zu gewährleisten.

<img width="1194" height="1244" alt="Domäne User remove" src="https://github.com/user-attachments/assets/38508d74-7ce9-423c-840b-369538cce36d" />

---

#### 6. NTFS-Berechtigungen gesetzt

**Ordner: `C:\Shares\Daten`**
- Gruppe: `EC2\intern`
- Berechtigung: **Read** (Lesen)

<img width="802" height="876" alt="intern gruppe daten premission" src="https://github.com/user-attachments/assets/f9a6f672-94dc-4a2a-8889-53e9b63cfb3a" />


**Ordner: `C:\Shares\Daten\Buchhaltung`**
- Gruppe: `EC2\Buchhaltung`
- Berechtigung: **Modify** (Ändern)

<img width="1300" height="1152" alt="buchhaltung premissions" src="https://github.com/user-attachments/assets/2392e429-f60b-47e3-bffa-fef6a8b14130" />


**Ordner: `C:\Shares\Daten\GL`**
- Gruppe: `EC2\GL`
- Berechtigung: **Modify** (Ändern)


<img width="1080" height="1142" alt="gl premissions" src="https://github.com/user-attachments/assets/f4c2b2f9-e3e6-439e-8586-3da405a556bd" />


**Ordner: `C:\Shares\Daten\Sekretariat`**
- Gruppe: `EC2\Sekretariat`
- Berechtigung: **Modify** (Ändern)


<img width="1186" height="1264" alt="sekretariat premissions" src="https://github.com/user-attachments/assets/5174f3c8-337c-4546-a2e7-d4308154aac0" />


**Ordner: `C:\Shares\Daten\Pool`**
- Gruppe: `EC2\intern`
- Berechtigung: **Modify** (Ändern)

<img width="1016" height="1144" alt="pool premissions" src="https://github.com/user-attachments/assets/ca198404-7ac8-4592-b261-f4f4cee80132" />


**Ordner: `C:\Shares\Aussendienst`**
- Gruppe: `EC2\extern`
- Berechtigung: **Read** (Lesen)

<img width="1732" height="1322" alt="extern premissions" src="https://github.com/user-attachments/assets/0f460e5b-e253-4313-9a4a-d68691366ee7" />


---

### Berechtigungsmatrix Übersicht

| Ordner | Gruppe | NTFS-Berechtigung | Freigabe-Berechtigung |
|--------|--------|-------------------|----------------------|
| Daten | intern | Read | Everyone: Change |
| Daten\Buchhaltung | Buchhaltung | Modify | - |
| Daten\GL | GL | Modify | - |
| Daten\Sekretariat | Sekretariat | Modify | - |
| Daten\Pool | intern | Modify | - |
| Aussendienst | extern | Read | Everyone: Change |

---

**Phase 3** 

***

```markdown
### Phase 3: ABE aktivieren & Berechtigungen testen

#### 1. Access-Based Enumeration (ABE) aktiviert


**Aktivierung über PowerShell:**

Auf dem Domain Controller wurde ABE für beide Freigaben aktiviert:

```powershell
Set-SmbShare -Name "Daten" -FolderEnumerationMode AccessBased
Set-SmbShare -Name "Aussendienst" -FolderEnumerationMode AccessBased
```

**Überprüfung:**
```powershell
Get-SmbShare | Select-Object Name, FolderEnumerationMode
```
<img width="1598" height="1144" alt="iSCIS downloaded" src="https://github.com/user-attachments/assets/b8e38827-f254-4a66-8ec3-06abf3722c0e" />

<img width="3576" height="1936" alt="ABE für Freigabe " src="https://github.com/user-attachments/assets/0cca1ee5-645d-4799-8ff0-c0ef28d8ae44" />

<img width="2880" height="1840" alt="ABE für Aussendienst Ordner" src="https://github.com/user-attachments/assets/5cbd9c96-3f5c-4be4-8c75-0b5bfeff35af" />

<img width="1863" height="1064" alt="ABE funktioniert" src="https://github.com/user-attachments/assets/60243428-c1bc-49bf-85cf-455627bee7f6" />


**Ergebnis:**
- Freigabe "Daten": AccessBased ✅
- Freigabe "Aussendienst": AccessBased ✅

---

#### 2. Berechtigungen getestet

**Test 1: Benutzer s.mueller (Sekretariat)**
- Login: `EC2\s.mueller`
- Zugriff auf: `\\dc\Daten`
- **Sichtbar:** Nur Ordner "Sekretariat" und "Pool" (ABE funktioniert)
- **Nicht sichtbar:** Buchhaltung, GL (kein Zugriff)
- **Schreibrechte in Sekretariat:** Erfolgreich getestet ✅

<img width="901" height="926" alt="Anmeldung auf Cleint für Test ABE" src="https://github.com/user-attachments/assets/09792128-8512-45ed-a88e-79df01671139" />

<img width="3592" height="1940" alt="File sharing sekretariat funktioniert" src="https://github.com/user-attachments/assets/c1629f02-e68e-4b35-b205-c14b7f36e3c5" />

<img width="3584" height="1902" alt="Sekretariat write file sharing funktioniert" src="https://github.com/user-attachments/assets/92d80502-f708-4165-8b30-ee04f1126112" />


---

**Test 2: Benutzer m.schmidt (Buchhaltung)**
- Login: `EC2\m.schmidt`
- Zugriff auf: `\\dc\Daten`
- **Sichtbar:** Nur "Buchhaltung" und "Pool"
- **Schreibrechte in Buchhaltung:** Erfolgreich ✅

<img width="3620" height="1950" alt="Buchhaltung User" src="https://github.com/user-attachments/assets/2e0d654f-dc1f-4af6-949f-b4974757957d" />
<img width="3616" height="1958" alt="Buchaltung Freigabe" src="https://github.com/user-attachments/assets/a678e83c-0fad-48b2-becd-322f706b034a" />
<img width="3594" height="1958" alt="Buchaltung Freigabe test" src="https://github.com/user-attachments/assets/aee20b55-56d3-4621-b8dc-e5f181cea9ea" />


---

**Test 3: Benutzer t.klein (Promoter - extern)**
- Login: `EC2\t.klein`
- Zugriff auf: `\\dc\Aussendienst`
- **Berechtigung:** Nur Lesezugriff (Read)
- **Schreibversuch:** Fehlgeschlagen (Access Denied) ✅

<img width="3590" height="1932" alt="Login mit Aussendienst user" src="https://github.com/user-attachments/assets/658b8551-9b80-4965-814f-3ab47ba766f8" />

<img width="2714" height="1502" alt="Aussendienst shared folder" src="https://github.com/user-attachments/assets/39b18284-23df-421e-bd9e-2c0430957c55" />
<img width="3558" height="1826" alt="Aussendienst shared folder read rechte" src="https://github.com/user-attachments/assets/01e5a41e-e794-4ee3-bc6d-55c64dadacc6" />
<img width="3550" height="1804" alt="Aussendienst shared folder read rechte test dc" src="https://github.com/user-attachments/assets/099ece67-b35f-4d42-9567-1a687facf3e8" />
<img width="3642" height="1932" alt="Aussendienst shared folder read rechte funktioniert" src="https://github.com/user-attachments/assets/4fcbd954-4902-4b24-a8bd-860b64b7f236" />


---

### Zusammenfassung der Tests

| Benutzer | Abteilung | Sichtbare Ordner | Schreibrechte | Ergebnis |
|----------|-----------|------------------|---------------|----------|
| s.mueller | Sekretariat | Sekretariat, Pool | Modify in Sekretariat | ✅ |
| m.schmidt | Buchhaltung | Buchhaltung, Pool | Modify in Buchhaltung | ✅ |
| t.klein | Promoter (extern) | Aussendienst | Nur Read | ✅ |

---
# Aufgabe 7: DIT & GPOs

## Übersicht
Implementierung von Group Policy Objects (GPOs) zur zentralen Verwaltung der Active Directory-Umgebung mit Passwort-Richtlinien, Netzlaufwerken, Desktop-Shortcuts, Druckern und Software-Verteilung.


---

## Teil 0: DIT-Diagramm

Das Directory Information Tree (DIT) wurde in draw.io erstellt und visualisiert die komplette AD-Struktur mit allen OUs, Benutzern und GPO-Verknüpfungen.

<img width="2366" height="1492" alt="image" src="https://github.com/user-attachments/assets/7d9157b5-b7aa-4eae-a007-6bc98db22631" />

---

## Teil 1: Passwort-Policy

**Ziel:** Sichere Passwort-Richtlinie für die gesamte Domain implementieren.

### Durchführung

1. Group Policy Management öffnen: `gpmc.msc`
2. Rechtsklick auf "Default Domain Policy" → Edit
3. Navigation: Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy

### Konfigurierte Einstellungen

- **Enforce password history:** 24 passwords
- **Maximum password age:** 90 days
- **Minimum password age:** 1 day
- **Minimum password length:** 12 characters
- **Password must meet complexity requirements:** Enabled
- **Store passwords using reversible encryption:** Disabled

<img width="2798" height="1348" alt="Deafault domain policy bearbeiten" src="https://github.com/user-attachments/assets/a2ab222c-3eff-44da-9dab-f26651e7aa1e" />

<img width="1594" height="1180" alt="Passwort Policy open" src="https://github.com/user-attachments/assets/d289d137-f162-47e7-b7fd-97348e363de3" />

<img width="1096" height="916" alt="Passwort Policy settings" src="https://github.com/user-attachments/assets/570335db-bc14-40ab-ba18-bf12ae5839eb" />

---

## Teil 2: Netzlaufwerke per GPO verteilen

**Ziel:** Automatische Zuweisung von Netzlaufwerken basierend auf Abteilungszugehörigkeit.

### Shares erstellen

1. Ordner erstellt unter `C:\Daten\`:
   - Pool, Buchhaltung, GL, Sekretariat, Promoter
2. Computer Management → Shared Folders → Shares → New Share
3. Jeden Ordner als Share mit gleichem Namen freigegeben
4. Berechtigungen: Everyone - Full Control

<img width="1932" height="1328" alt="shares ordner erstellt" src="https://github.com/user-attachments/assets/b05f400e-c1c9-46ba-9b2c-f8e7fbbfc21d" />


### GPO für Pool-Laufwerk (alle Mitarbeiter)

1. `gpmc.msc` → Group Policy Objects → New
2. Name: `GPO-Drive-Pool-All`
3. Rechtsklick → Edit
4. User Configuration → Preferences → Windows Settings → Drive Maps → New → Mapped Drive
5. Konfiguration:
   - Action: Create
   - Location: `\\1.ec2.yenul.m159\Pool`
   - Label as: Pool
   - Drive Letter: P:
   - Reconnect: ✓
6. GPO mit Domain verknüpft

### Abteilungs-Laufwerke

Für jede Abteilung wurde eine eigene GPO erstellt und mit der entsprechenden OU verknüpft:

| GPO Name | Share Path | Laufwerk | Verknüpft mit |
|----------|-----------|----------|---------------|
| GPO-Drive-Buchhaltung | `\\1.ec2.yenul.m159\Buchhaltung` | B: | OU=Buchhaltung |
| GPO-Drive-GL | `\\1.ec2.yenul.m159\GL` | G: | OU=GL |
| GPO-Drive-Sekretariat | `\\1.ec2.yenul.m159\Sekretariat` | S: | OU=Sekretariat |
| GPO-Drive-Promoter | `\\1.ec2.yenul.m159\Promoter` | R: | OU=Promoter |

<img width="1486" height="1054" alt="neue gpo" src="https://github.com/user-attachments/assets/c2bead9e-6981-4124-9dd4-8ce9d5ebac6c" />

<img width="1568" height="1182" alt="Pool Laufwerk erstellt" src="https://github.com/user-attachments/assets/f7cb332e-e55c-4dc6-bbd5-e58ce1fe4eba" />

<img width="1544" height="1106" alt="GPO zu Domain verlinken" src="https://github.com/user-attachments/assets/3197ddc6-b1d9-4422-9c7e-a70937479f1d" />

<img width="532" height="986" alt="GPOs und mapped Drives erstellt" src="https://github.com/user-attachments/assets/67a41a50-ae21-4a2f-8da5-4621610b149e" />

<img width="3000" height="1260" alt="shortcut erstelltn" src="https://github.com/user-attachments/assets/313d999c-59e2-481c-9cad-28c4755caf2c" />

<img width="2472" height="1944" alt="gpo intern zu gruppe verteilen" src="https://github.com/user-attachments/assets/30ce936a-7f82-4459-88b4-c873fd98d67e" />

---

## Teil 3: Desktop-Shortcut zu Web-CRM

**Ziel:** Automatische Desktop-Verknüpfung zur Web-CRM-Anwendung für interne Mitarbeiter.

### Durchführung

1. GPO erstellen: `GPO-Desktop-Shortcut-Intern`
2. Edit → User Configuration → Preferences → Windows Settings → Shortcuts → New → Shortcut
3. Konfiguration:
   - Action: Create
   - Name: Web-CRM
   - Target type: URL
   - Location: Desktop
   - Target URL: `https://crm.webapp.ch`
4. Security Filtering: Buchhaltung, GL, Sekretariat
5. GPO mit OU "Intern" verknüpft

<img width="3614" height="1970" alt="web crm auf desktop test" src="https://github.com/user-attachments/assets/48509ef7-3711-4e16-8a61-60626ac3b89b" />


---

## Teil 4: WMI-Filter

WMI-Filter ermöglichen GPO-Anwendung basierend auf System-Eigenschaften.

**Beispiel - Filter für Windows 10/11:**
1. `gpmc.msc` → WMI Filters → New
2. Name: `Filter-Windows10-11`
3. Query: `SELECT * FROM Win32_OperatingSystem WHERE Version LIKE "10.%" OR Version LIKE "11.%"`
4. Filter der gewünschten GPO zuweisen

<img width="3076" height="1658" alt="WMI Filter erstellt" src="https://github.com/user-attachments/assets/bf3533df-1ef2-428b-b502-978fb2bf6306" />

---

## Teil 5: Drucker per GPO verteilen

**Ziel:** Abteilungsspezifische Drucker automatisch bereitstellen.

### Drucker erstellen

1. Settings → Devices → Printers & scanners → Add printer
2. "Add a local printer or network printer with manual settings"
3. Port: FILE: (Print to File)
4. Driver: Generic / Text Only
5. Drucker erstellt und freigegeben:
   - Drucker-Buchhaltung
   - Drucker-GL
   - Drucker-Sekretariat
   - Drucker-Promoter

### GPO für Drucker-Verteilung

Für jede Abteilung wurde eine Drucker-GPO erstellt:

1. GPO erstellen (z.B. `GPO-Printer-Buchhaltung`)
2. Edit → User Configuration → Preferences → Control Panel Settings → Printers
3. New → Shared Printer
4. Konfiguration:
   - Action: Create
   - Share path: `\\1.ec2.yenul.m159\Drucker-[Abteilung]`
   - Set as default printer: ✓
5. GPO mit entsprechender OU verknüpft

| GPO Name | Drucker-Share | Verknüpft mit |
|----------|--------------|---------------|
| GPO-Printer-Buchhaltung | `\\1.ec2.yenul.m159\Drucker-Buchhaltung` | OU=Buchhaltung |
| GPO-Printer-GL | `\\1.ec2.yenul.m159\Drucker-GL` | OU=GL |
| GPO-Printer-Sekretariat | `\\1.ec2.yenul.m159\Drucker-Sekretariat` | OU=Sekretariat |
| GPO-Printer-Promoter | `\\1.ec2.yenul.m159\Drucker-Promoter` | OU=Promoter |

<img width="2284" height="1680" alt="drucker " src="https://github.com/user-attachments/assets/849f74e4-0c15-4f4f-85dc-1ffc6b3ec979" />

<img width="2128" height="1702" alt="drucker gl" src="https://github.com/user-attachments/assets/4121957d-1b7a-46a0-b0ee-7fd525ee25c0" />

<img width="1770" height="1332" alt="Drucker sekretariat" src="https://github.com/user-attachments/assets/cfb3c40c-30c1-4767-b6c6-0eafb7718ec5" />

<img width="3032" height="1826" alt="Drucker Promoter" src="https://github.com/user-attachments/assets/fcb35079-dbbf-44c3-9227-74ccdb19bf0b" />

<img width="1630" height="1210" alt="printer gpos" src="https://github.com/user-attachments/assets/ac5800fb-9f10-4ffd-bd29-e92d94d4857f" />

<img width="3610" height="1918" alt="Drucker test " src="https://github.com/user-attachments/assets/edfec7ee-1a1d-4129-ac2b-9461c345eb0c" />

---

## Teil 6: Software-Installation (Notepad++)

**Ziel:** Automatische Installation von Notepad++ im Sekretariat.

### Vorbereitung

1. Notepad++ MSI-Installer heruntergeladen (x64 Version)
2. Software-Share erstellt: `C:\SoftwarePackages`
3. Als `\\1.ec2.yenul.m159\SoftwarePackages` freigegeben
4. MSI-Datei im Share abgelegt

### GPO erstellen

1. GPO erstellen: `GPO-Software-Notepad`
2. Edit → Computer Configuration → Policies → Software Settings → Software installation
3. New → Package → MSI-Datei ausgewählt
4. Deployment: Assigned
5. GPO mit OU "Sekretariat" verknüpft

### Anwendung

Software-Installation erfolgt beim Computer-Start (nicht bei `gpupdate /force`). Client-Computer muss neu gestartet werden.

<img width="2804" height="1582" alt="Software GPO" src="https://github.com/user-attachments/assets/918be6f3-9d44-49c8-8560-62208d664b4c" />

<img width="3356" height="1350" alt="GPO Software verknüpft" src="https://github.com/user-attachments/assets/088eee25-6a88-4209-8087-f0e4bbc09c22" />
<img width="3636" height="1972" alt="Software verteilt test" src="https://github.com/user-attachments/assets/b42c1643-5c23-4243-b352-e5ce55bf62ab" />

---

## Überprüfung

### GPO-Anwendung erzwingen

