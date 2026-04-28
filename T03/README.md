# T03: Servidor de Fitxers - FoodLogistic 🚚📂

![GitHub methodology](https://img.shields.io/badge/Methodology-FSRM%20%7C%20NTFS%20%7C%20SMB-blue)
![Role](https://img.shields.io/badge/Role-File%20Server%20Administrator-orange)

## 1. Introducció
Quan el volum de negoci creix, també ho fa el volum de dades. A **FoodLogistic**, la informació s'ha compartimentat excessivament, provocant una pèrdua de la visió global de l'empresa degut a que cada departament guardava la documentació de forma local.

### Objectiu del Projecte
Implementar una infraestructura de fitxers **segura, organitzada i amb control d'espai**, mitjançant:
* Permisos **NTFS/SMB**.
* **Quotes i filtratge** de fitxers (FSRM).
* Administració mitjançant **Explorador de fitxers, Server Manager i PowerShell**.

---

## 2. Descripció de l'Activitat

### 2.1. Preparació i Seguretat de Grups (AD)
Configuració prèvia a l'Active Directory sobre el domini `foodlogistic.test`:
* **Estructura d'OUs:** Disseny d'una jerarquia coherent i justificada.
* **Grups de Seguretat:**
    * `Administracio`: Gestió de factures i albarans.
    * `Transport`: Xofers i caps de flota.
    * `Direccio`: Gerència.

### 2.2. Implementació de Recursos Compartits
S'han utilitzat tres mètodes d'implementació per a les diferents estructures de carpetes:

| Carpeta | Mètode d'Administració | Configuració Específica |
| :--- | :--- | :--- |
| **Public** | Explorador d'arxius | Permisos SMB (Lectura) i NTFS (Modificació). |
| **Operacions** | Server Manager (FSSM) | Access-Based Enumeration (ABE). Accés exclusiu grup `Transport`. |
| **Confidencial** | PowerShell (Avançat) | Recurs ocult `Direccio$`, ABE habilitat per cmdlet, Unitat de xarxa `Z:` via GPO. |

> **Nota sobre el Mètode PowerShell:** S'ha optat per la implementació avançada utilitzant el cmdlet `New-SmbShare` per habilitar l'Access-Based Enumeration i la gestió de permisos d'accés exclusius per al grup `Direccio`.

### 2.3. Control d'Emmagatzematge (FSRM i Quotes)
Per evitar l'ús inadequat del disc amb fitxers personals, s'apliquen les següents restriccions:

* **Quotes NTFS (Volum):**
    * Límit per defecte de **500 MB** per a nous usuaris a la unitat de dades.
* **FSRM (Carpeta):**
    * **Quota a `Public`:** 200 MB (Hard Quota).
    * **Notificació:** Avís personalitzat al 90% de capacitat.
    * **Filtratge a `Operacions`:** Bloqueig actiu d'executables (`.exe`, `.msi`) i fitxers multimèdia (àudio/vídeo).

---

## 3. Verificació i Auditoria
Proves realitzades des de clients Windows 10/11:
1.  **Validació d'accés:** Comprovació de visibilitat de recursos segons el grup de l'usuari.
2.  **Test de Filtratge:** Intent de còpia de fitxers executables a la carpeta `Operacions`.
3.  **Filtratge Actiu:** Verificació del bloqueig d'executables fins i tot canviant l'extensió a `.txt`.

---

## 4. Contingut del Lliurable
L'informe tècnic detallat inclou:
* **Resum de Configuració:** Taula resum amb noms, camins UNC i permisos.
* **Evidències:** Captures de pantalla de les configuracions en AD, Server Manager i consola de PowerShell.
* **Proves de funcionament:** Demostració del correcte funcionament de les quotes i restriccions des del costat del client.

---
**Material de suport:** *0224 SOX. Material UD8: AA2 (Moodle)*
