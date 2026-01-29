# M159 - Projekt-Setup-Sheet 

In diesem Dokument werden sämtliche Angaben sowie Passwörter, welche Sie für die Installation Ihrer Umgebung benötigen, festgehalten.

## 1. Übersicht Umgebung

Diese Umgebung umfasst:

- **1x Windows Server (DC)** auf AWS EC2 im PUBLIC Subnetz
- **1x Windows Server (Client)** auf AWS EC2 im PUBLIC Subnetz
- **1x Windows Server (Admin Center)** auf AWS EC2 im PUBLIC Subnetz
- **AWS Managed AD** mit Trust zur On-Premises (EC2) AD in PRIVATE Subnets
- **Entra Connect** zur Synchronisation mit Entra ID
- **Lokale AD Domain** (zu Beginn), später öffentliche Domain als UPN

## 2. Allgemeine Angaben

| Feld | Wert |
|------|------|
| Vorname | Yenul |
| Nachname | Jayawardhana |
| Klasse | M159 |
| Dokumentation (GIT-Repository-Link) | https://github.com/yja06/m159 |

## 3. Ressourcen

| Feld | Wert |
|------|------|
| Active Directory Second-Level-Domäne | yenul.m159 |
| Geplante öffentliche Domain (UPN) | yenul159.v6.rocks |
| Azure Education Account | jayawardhanayenul@gmail.com |
| Azure Education Account Passwort | [privat] |

## 4. AWS VPC Setup (KORRIGIERT)

**Hinweis:** DC, Client und Admin Center liegen in PUBLIC Subnets (mit Elastic IPs für RDP). AWS Managed AD liegt in PRIVATE Subnets.

| Komponente | VPC-ID | CIDR | Name | Availability Zone |
|-----------|--------|------|------|-------------------|
| VPC | - | 10.0.0.0/16 | M159-vpc-yenul | - |
| Public Subnet 1a (DC) | - | 10.0.1.0/24 | M159-public1a-yenul | us-east-1a |
| Public Subnet 1b (Client/Admin) | - | 10.0.2.0/24 | M159-public1b-yenul | us-east-1b |
| Private Subnet 2a (AWS Managed AD) | - | 10.0.100.0/24 | M159-private2a-yenul | us-east-1a |
| Private Subnet 2b (AWS Managed AD Backup) | - | 10.0.101.0/24 | M159-private2b-yenul | us-east-1b |

## 5. AWS Sicherheitsgruppen

### Sicherheitsgruppe für Domain Controller (M159-DC-SG)

| Regeltyp | Port(e) | Protokoll | Quelle |
|----------|---------|-----------|--------|
| RDP | 3389 | TCP | 0.0.0.0/0 |
| LDAP | 389 | TCP/UDP | 10.0.0.0/16 |
| LDAPS | 636 | TCP | 10.0.0.0/16 |
| Kerberos | 88 | TCP/UDP | 10.0.0.0/16 |
| SMB | 445 | TCP | 10.0.0.0/16 |
| DNS | 53 | TCP/UDP | 10.0.0.0/16 |
| RPC | 135 | TCP | 10.0.0.0/16 |
| RPC Ephemeral | 49152-65535 | TCP/UDP | 10.0.0.0/16 |
| ICMP | Alle | ICMP | 10.0.0.0/16 |
| Global Catalog | 3268 | TCP | 10.0.0.0/16 |
| Global Catalog SSL | 3269 | TCP | 10.0.0.0/16 |
| Kerberos PW Change | 464 | TCP/UDP | 10.0.0.0/16 |

### Sicherheitsgruppe für Clients (M159-Client-SG)

| Regeltyp | Port(e) | Protokoll | Beschreibung | Quelle |
|----------|---------|-----------|--------------|--------|
| RDP | 3389 | TCP | Remote Desktop | 0.0.0.0/0 |
| Kerberos | 88 | TCP/UDP | Kerberos Authentication | 10.0.0.0/16 |
| RPC | 135 | TCP | RPC Endpoint Mapper | 10.0.0.0/16 |
| NetBIOS | 139 | TCP | NetBIOS Session Service | 10.0.0.0/16 |
| LDAP | 389 | TCP | LDAP | 10.0.0.0/16 |
| DNS | 53 | UDP | DNS | 10.0.0.0/16 |
| SMB | 445 | TCP | SMB/CIFS Dateifreigabe | 10.0.0.0/16 |
| RPC Ephemeral | 49152-65535 | TCP/UDP | RPC Ephemeral Ports | 10.0.0.0/16 |
| ICMP | Alle | ICMP | Ping etc. | 10.0.0.0/16 |

### Sicherheitsgruppe für AWS Managed AD (M159-AWS-AD-SG)

| Regeltyp | Port(e) | Protokoll | Quelle |
|----------|---------|-----------|--------|
| LDAP | 389 | TCP/UDP | 10.0.0.0/16 |
| LDAPS | 636 | TCP | 10.0.0.0/16 |
| Kerberos | 88 | TCP/UDP | 10.0.0.0/16 |
| SMB | 445 | TCP | 10.0.0.0/16 |
| DNS | 53 | TCP/UDP | 10.0.0.0/16 |
| RPC | 135 | TCP | 10.0.0.0/16 |
| RPC Ephemeral | 49152-65535 | TCP/UDP | 10.0.0.0/16 |
| Kerberos PW Change | 464 | TCP/UDP | 10.0.0.0/16 |
| Global Catalog | 3268 | TCP | 10.0.0.0/16 |

## 6. Active Directory Umgebung

### On-Premises Active Directory (AWS EC2)

| Feld | Wert |
|------|------|
| Active Directory Third-Level-Domäne | ec2.yenul.m159 |
| Öffentlicher UPN Suffix (später) | yenul159.v6.rocks |
| Domänenadministrator | Administrator |
| Kennwort Domänenadministrator | StrengesPasswort!DC-2025 |
| Kennwort-Demote (Herunterstufen) | StrengesPasswort!Demote-2025 |

### Azure AD (Entra ID)

| Feld | Wert |
|------|------|
| Entra AD Tenant Name | yenulm159.onmicrosoft.com |
| Azure Administrator (UPN) | admin@yenulm159.onmicrosoft.com |
| Kennwort Azure Administrator | StrengesPasswort!Azure-2025 |
| Entra Connect Server (Name) | connect.ec2.yenul.m159 |

### AWS Managed AD

| Feld | Wert |
|------|------|
| Active Directory Third-Level-Domäne | aws.yenul.m159 |
| Trust Typ | Tree Root Trust |
| AWS Managed Admin User | admin |
| AWS Managed Admin Passwort | StrengesPasswort!AWS-AD-2025 |
| DNS-Server 1 | 10.0.100.10 |
| DNS-Server 2 | 10.0.1.10 (OnPrem DC) |
| Trust Passwort | StrengesPasswort!Trust-2025 |
| Subnetz 1 | M159-private2a-yenul (10.0.100.0/24) |
| Subnetz 2 | M159-private2b-yenul (10.0.101.0/24) |

## 7. EC2-Instanzen (KORRIGIERT)

| Komponente | FQDN | Private IP | Subnet | Availability Zone | Elastic IP | DNS 1 | DNS 2 | Lokaler Admin | Kennwort |
|-----------|------|-----------|--------|-------------------|------------|-------|-------|---------------|----------|
| On-Prem AD DC | dc1.ec2.yenul.m159 | 10.0.1.10 | Public 1a (10.0.1.0/24) | us-east-1a | 52.6.248.6 | 10.0.1.10 | 10.0.100.10 | Administrator | Tbz12344$ |
| Windows Server (Client) | client1.ec2.yenul.m159 | 10.0.2.20 | Public 1b (10.0.2.0/24) | us-east-1b | 100.25.38.57 | 10.0.1.10 | 10.0.100.10 | Administrator | Tbz12344$ |
| Windows Server Admin Center | admincenter.ec2.yenul.m159 | 10.0.2.30 | Public 1b (10.0.2.0/24) | us-east-1b |  | 10.0.1.10 | 10.0.100.10 | Administrator | Tbz12344$ |

## 8. Abteilungen & Benutzer

| Abteilung | Name der Abteilung | Benutzername | Vorname | Nachname | Kennwort | Bereiche |
|-----------|-------------------|--------------|---------|----------|----------|----------|
| 1 | Sekretariat | sek-anna | Anna | Frei | Sekretariat!2025 | intern |
| 2 | Buchhaltung | buch-luca | Luca | Steiner | Buchhaltung!2025 | intern |
| 3 | GL | gl-sandra | Sandra | Keller | GL!2025 | intern |
| 4 | Promoter | promo-max | Max | Weber | Promoter!2025 | extern |


## 10. Hinweise

- Die lokale Domain **ec2.yenul.m159** wird zuerst eingerichtet
- Der öffentliche UPN **yenul159.v6.rocks** wird später als alternativer UPN-Suffix in der lokalen AD und in Entra ID konfiguriert
- **PUBLIC Subnets** (10.0.1.0/24, 10.0.2.0/24): DC, Client, Admin Center mit Elastic IPs für RDP-Zugriff von außen
- **PRIVATE Subnets** (10.0.100.0/24, 10.0.101.0/24): AWS Managed AD (nur interne Kommunikation)
- Die Sicherheitsgruppen ermöglichen nur RDP von außen. Alle anderen AD-relevanten Ports sind auf die internen Netze des VPC beschränkt
- **Wichtig:** Nach jedem Schritt einen Snapshot der EC2-Instanzen erstellen!
- **Backup:** Täglich nach Schulende VM-Snapshot sichern
