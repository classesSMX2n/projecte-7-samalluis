# T03: Servidor de Fitxers - FoodLogistic 🚚📂

![GitHub methodology](https://img.shields.io/badge/Methodology-FSRM%20%7C%20NTFS%20%7C%20SMB-blue)
![Role](https://img.shields.io/badge/Role-File%20Server%20Administrator-orange)

## 📝 Introducció
A mesura que el volum de negoci de **FoodLogistic** ha crescut, la informació s'ha compartimentat excessivament, generant una pèrdua de la visió global. Fins ara, cada departament guardava la documentació de forma local.

Aquest projecte té com a objectiu implementar una **infraestructura de fitxers segura, organitzada i centralitzada** utilitzant Windows Server. S'apliquen controls estrictes mitjançant permisos NTFS/SMB, quotes d'espai i filtratge de fitxers (FSRM).

---

## 🛠️ Administració Utilitzada
S'han demostrat dominis en les tres vies d'administració de Windows Server:
1.  **Explorador de fitxers** (Configuració manual).
2.  **Server Manager** (FSSM - File and Storage Services).
3.  **PowerShell** (Automatització i configuració avançada).

---

## 🚀 Fites del Projecte

### 1. Preparació de l'Active Directory (AD)
S'ha dissenyat una estructura d'Unitats Organitzatives (OU) dins del domini `foodlogistic.test` per gestionar els següents grups de seguretat:
* **Administracio**: Gestió de factures i albarans.
* **Transport**: Xofers i caps de flota.
* **Direccio**: Gerència de l'empresa.

### 2. Implementació de Recursos Compartits
| Carpeta | Mètode | Accés / Permisos | Característiques |
| :--- | :--- | :--- | :--- |
| **Public** | Explorador d'arxius | Tothom (SMB: Lectura / NTFS: Modificació) | Accés general |
| **Operacions** | Server Manager | Només grup **Transport** | Access-Based Enumeration (ABE) |
| **Confidencial** | PowerShell Avançat | Només grup **Direccio** | ABE, recurs ocult, Mapeig unitat Z: via GPO |

> [!TIP]
> Per a la carpeta **Confidencial** s'ha utilitzat el mètode de PowerShell avançat amb el cmdlet `New-SmbShare` per optimitzar la visibilitat i seguretat.

### 3. Control d'Emmagatzematge (FSRM & Quotes)
Per evitar l'ús inadequat del disc amb arxius personals, s'han establert les següents polítiques:

* **Quotes NTFS (Volum):** Límit per defecte de **500 MB** per a nous usuaris a la unitat de dades.
* **Quotes de Carpeta (FSRM):** * Ubicació: `/Public`
    * Límit: **200 MB (Hard Quota)**.
    * Notificació: Avís personalitzat al 90% de capacitat.
* **Filtrat de Fitxers:** * Ubicació: `/Operacions`
    * Bloqueig: Executables (`.exe`, `.msi`) i fitxers d'àudio/vídeo.

---

## 🔍 Verificació i Auditoria
Es realitzen les següents proves des d'un client Windows 10/11:
1.  **Comprovació de visibilitat:** Validació de l'Access-Based Enumeration segons l'usuari loguejat.
2.  **Prova de filtratge actiu:** Intent de còpia de fitxers `.exe` i prova de canvi d'extensió (bypass check).
3.  **Mapeig de xarxa:** Verificació de l'aparició automàtica de la unitat **Z:** per al grup de Direcció.

---

## 📂 Contingut del Lliurament
L'informe tècnic adjunt inclou:
- [ ] **Resum de Configuració:** Taula detallada amb camins UNC i mètodes.
- [ ] **Evidències:** Captures de pantalla de la configuració de rols i permisos.
- [ ] **Proves de funcionament:** Logs i captures del costat del client validant les restriccions.

---
**Assignatura:** 0224 SOX - Unitat Didàctica 8
**Entorn:** Windows Server 2022 / Windows 10-11 Client