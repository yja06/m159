# M159 - Projekt-Setup-Sheet

In diesem Dokument werden saemtliche Angaben sowie Passwoerter, welche Sie fuer die Installation Ihrer Umgebung benoetigen, festgehalten.

![Modul_159_Architekturdiagramm](Modul_159_Architekturdiagramm.drawio.svg)

---

## 1. Uebersicht Umgebung

Diese Umgebung umfasst:

1x Windows Server (DC) auf AWS EC2  
1x Windows Server (Client) auf AWS EC2 (da AWS keine Windows Clients anbietet)  
1x Windows Server (Admin Center) auf AWS EC2 zur Verwaltung der AWS Managed AD  
AWS Managed AD mit Trust zur On-Premises (EC2) AD  
Entra Connect zur Synchronisation mit Entra ID sowie Entra AD  
Lokale AD Domain (zu Beginn), spaeter oeffentliche Domain als UPN

---

## 2. Allgemeine Angaben

| Feld                                | Wert                                           |
| ----------------------------------- | ---------------------------------------------- |
| Vorname                             | Yenul                                          |
| Nachname                            | Jayawardhana                                   |
| Klasse                              | M159                                           |
| Dokumentation (GIT-Repository-Link) | https://github.com/yja06/m159                  |

---

## 3. Ressourcen

| Feld                                                         | Wert                         |
| ------------------------------------------------------------ | ---------------------------- |
| Active Directory Second-Level-Domäne                        | yenul.m159                    |
| Geplante öffentliche Domain (UPN) -> dynv6                  | yenul159.dynv6.net             |
| Azure Education Account mit 80$                              | jayawardhanayenul@gmail.com  |
| Azure Education Account Passwort                             | privat                       |

Hinweis: Das echte Azure Passwort besser im Passwort Manager speichern und hier nur als Schema dokumentieren.

---

## 4. AWS VPC Setup

Hinweis:  
Alle Instanzen liegen in einem oeffentlichen oder privaten Subnetz und sind ueber RDP (Port 3389) von aussen erreichbar.  
Alle weiteren Ports sind nur innerhalb des VPCs offen.

| Komponente                      | VPC-ID                | CIDR         | Name                              |
| ------------------------------- | --------------------- | ----------- | --------------------------------- |
| VPC                             |                       | 10.0.0.0/16  | M159-vpc-yenul                    |
| M159-subnet-private1-us-east-1a |                       | 10.0.128.0/20 | M159-private1-yenul-us-east-1a    |
| M159-subnet-private2-us-east-1b |                       | 10.0.144.0/20 | M159-private2-yenul-us-east-1b    |
| M159-subnet-public1-us-east-1a  |                       | 10.0.0.0/20   | M159-public1-yenul-us-east-1a     |
| M159-subnet-public2-us-east-1b  |                       | 10.0.16.0/20  | M159-public2-yenul-us-east-1b     |

---

## 5. AWS Sicherheitsgruppen

### Sicherheitsgruppe fuer Domain Controller

| Regeltyp                     | Port(e)                 | Quelle        |
| ---------------------------- | ----------------------- | ------------- |
| RDP                          | 3389 (TCP)              | 0.0.0.0/0     |
| LDAP                         | 389 (TCP/UDP)           | 10.0.0.0/16   |
| LDAPS                        | 636 (TCP)               | 10.0.0.0/16   |
| Kerberos                     | 88 (TCP/UDP)            | 10.0.0.0/16   |
| SMB                          | 445 (TCP)               | 10.0.0.0/16   |
| DNS                          | 53 (TCP/UDP)            | 10.0.0.0/16   |
| RPC                          | 135, 49152-65535 (TCP)  | 10.0.0.0/16   |
| ICMP                         | Alle                    | 10.0.0.0/16   |
| Global Catalog               | 3268 (TCP)              | 10.0.0.0/16   |
| Global Catalog SSL           | 3269 (TCP)              | 10.0.0.0/16   |
| Kerberos Password Change/Set | 464 (TCP/UDP)           | 10.0.0.0/16   |

### Sicherheitsgruppe fuer Clients

| Regeltyp | Port(e)     | Beschreibung                             | Quelle                                           |
| -------- | ----------- | ---------------------------------------- | ------------------------------------------------ |
| RDP      | 3389        | Remote Desktop                           | 0.0.0.0/0                                        |
| TCP      | 88          | Kerberos Authentication                  | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| TCP      | 135         | RPC Endpoint Mapper                      | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| TCP      | 139         | NetBIOS Session Service                  | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| TCP      | 389         | LDAP                                     | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| UDP      | 53          | DNS                                      | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| TCP      | 445         | SMB/CIFS (Dateifreigabe, AD Operationen) | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| TCP      | 49152-65535 | RPC Ephemeral Ports                      | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |
| ICMP     | Alle        | Ping etc.                                | 10.0.0.0/20, 10.0.128.0/20, 10.0.144.0/20        |

---

## 6. Active Directory Umgebung

### On-Premises Active Directory (AWS EC2)

| Feld                                  | Wert                  |
| ------------------------------------- | --------------------- |
| Active Directory Third-Level-Domaene-1 | ec2.yenul.m159        |
| Oeffentlicher UPN Suffix (spaeter)    | yenul159.v6.rocks     |
| Domaenenadministrator                 | Administrator         |
| Kennwort Domaenenadministrator        | StrengesPasswort!DC-2025 |
| Kennwort-Demote (Herunterstufen)      | StrengesPasswort!Demote-2025 |

### Azure AD (Entra ID)

| Feld                         | Wert                                   |
| ---------------------------- | -------------------------------------- |
| Entra AD Tenant Name         | yenulm159.onmicrosoft.com              |
| Azure Administrator (UPN)    | admin@yenulm159.onmicrosoft.com        |
| Kennwort Azure Administrator | StrengesPasswort!Azure-2025           |
| Entra Connect Server (Name)  | connect.ec2.yenul.m159                 |

### AWS Managed AD

| Feld                                  | Wert                                                 |
| ------------------------------------- | ---------------------------------------------------- |
| Active Directory Third-Level-Domaene-2 | aws.yenul.m159                                      |
| Trust Typ                             | Tree Root Trust                                      |
| AWS Managed Admin User                | admin                                                |
| AWS Managed Admin Passwort            | StrengesPasswort!AWS-AD-2025                        |
| IP-Adresse                            | 10.0.145.10                                         |
| DNS-Server 1                          | 10.0.145.10                                         |
| DNS-Server 2                          | 10.0.129.10 (OnPrem DC)                             |
| Trust Passwort                        | StrengesPasswort!Trust-2025                         |
| Subnetz 1                             | M159-subnet-private1-us-east-1a (10.0.128.0/20)     |
| Subnetz 2                             | M159-subnet-private2-us-east-1b (10.0.144.0/20)     |

---

## 7. EC2 Instanzen

| Komponente                                       | FQDN                          | Elastic IP        | Private IP (CIDR) | Subnetz                         | DNS-Server 1 | DNS-Server 2 | Lokaler Admin | Kennwort                       |
| ------------------------------------------------ | ----------------------------- | ----------------- | ----------------- | ------------------------------- | ------------ | ------------ | ------------- | ------------------------------ |
| IaaS / OnPrem AD DC                              | dc1.ec2.yenul.m159            |                   | 10.0.129.10       | M159-subnet-private1-us-east-1a | 127.0.0.1    | 10.0.145.10  | Administrator | StrengesPasswort!DC-Local-2025 |
| Windows Server (Client)                          | client1.ec2.yenul.m159        | 44.198.134.36     | 10.0.129.20       | M159-subnet-public1-us-east-1a  | 10.0.129.10  | 10.0.145.10  | Administrator | StrengesPasswort!Client-2025   |
| Managed AWS EC2 DC                               | awsdc1.aws.yenul.m159         |                   | 10.0.145.10       | M159-subnet-private2-us-east-1b | 127.0.0.1    | 10.0.129.10  | admin         | StrengesPasswort!AWS-AD-2025   |
| Windows Server Admin Center (Managed AWS EC2 DC) | admincenter.aws.yenul.m159    | 44.198.140.50     | 10.0.0.50         | M159-subnet-public2-us-east-1b  | 10.0.145.10  | 10.0.129.10  | Administrator | StrengesPasswort!WAC-2025      |

---

## 8. Abteilungen und Benutzer

| Abteilung | Name der Abteilung | Benutzername     | Vorname | Nachname | Kennwort                     | Bereiche |
| --------- | ------------------ | ---------------- | ------- | -------- | ---------------------------- | -------- |
| 1         | Sekretariat        | sek-anna         | Anna    | Frei     | Sekretariat!2025             | intern   |
| 2         | Buchhaltung        | buch-luca        | Luca    | Steiner  | Buchhaltung!2025             | intern   |
| 3         | GL                 | gl-sandra        | Sandra  | Keller   | GL!2025                      | intern   |
| 4         | Promoter           | promo-max        | Max     | Weber    | Promoter!2025                | extern   |

---

## 9. Python App Registration (Entra ID)

| Name                    | Wert                               |
| ----------------------- | ---------------------------------- |
| Directory (tenant) ID   | wird nach Erstellung eingetragen   |
| Application (client) ID | wird nach Erstellung eingetragen   |
| Client Secret ID        | wird nach Erstellung eingetragen   |

---

## 10. Hinweise

Die lokale Domain `ec2.yenul.m159` wird zuerst eingerichtet.  
Der oeffentliche UPN `yenul159.v6.rocks` wird spaeter als alternativer UPN Suffix in der lokalen AD und in Entra ID konfiguriert.  
Die IP Bereiche und Subnetze sind so gewaehlt, dass OnPrem AD, AWS Managed AD und Admin Center klar getrennt sind, aber im gleichen VPC liegen.  
Die Sicherheitsgruppen ermoeglichen nur RDP von aussen. Alle anderen AD relevanten Ports sind auf die internen Netze des VPC eingeschraenkt.
