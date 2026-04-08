# 📘 Guia d'Implementació Tècnica: Servidor de Fitxers

Aquesta guia detalla el procés de configuració de la infraestructura de dades per a **FoodLogistic**, assegurant un entorn escalable, segur i controlat.

---

## 🏗️ 1. Disseny de l'Active Directory
Abans de la implementació del servidor de fitxers, s'ha preparat l'estructura lògica a `foodlogistic.test`.

### Estructura d'Unitats Organitzatives (OU)
* **OU_FoodLogistic**: Contenidor principal.
    * **OU_Groups**: Conté els grups `Administracio`, `Transport` i `Direccio`.
    * **OU_Users**: Usuaris nominals vinculats als seus respectius departaments.

> **Justificació:** Aquesta separació permet aplicar GPOs de seguretat i mapeig de unitats de forma granular segons el rol de l'empleat.


---

## 🛠️ 2. Configuració de Recursos Compartits

### A. Recurs: Public (Explorador de fitxers)
Configuració manual per a l'intercanvi ràpid de fitxers no crítics.
* **Permisos SMB:** `Everyone` -> Read.
* **Permisos NTFS:** `Users` -> Modify.
* **Resultat:** L'usuari pot escriure i editar, però el control de la sessió de xarxa és restrictiu.

### B. Recurs: Operacions (Server Manager)
Implementació mitjançant **FSSM (File and Storage Sharing Services)**.
* **Protocol:** SMB Quick.
* **Funcionalitat clau:** **Access-Based Enumeration (ABE)**.
* **Seguretat:** Només el grup `Transport` té visibilitat i accés. Els altres usuaris ni tan sols veuran la carpeta.

### C. Recurs: Confidencial (PowerShell Avançat)
S'ha optat pel **Mètode D** per a una major automatització i seguretat.

```powershell
# Creació del directori i share amb ABE habilitat
New-Item -Path "D:\Shares\Confidencial" -ItemType Directory
New-SmbShare -Name "Direccio" -Path "D:\Shares\Confidencial" -FolderEnumerationMode AccessBased -FullAccess "foodlogistic\Direccio"

# Mapeig automàtic via GPO
# Action: Update | Drive Letter: Z: | Path: \\FS01\Direccio
````

## 🛡️ 3. Polítiques de Control (FSRM & Quotes)

### 📊 Quotes de Disc

Per evitar l'esgotament de l'emmagatzematge per dades no professionals, s'han establert dos nivells de control:

* **Quotes NTFS (Nivell de Volum):**
    * **Ubicació:** Propietats de la unitat `D:`.
    * **Límit:** 500 MB.
    * **Acció:** Denegar espai de disc a usuaris que superin el límit.
* **Quotes de Carpeta (FSRM):**
    * **Ubicació:** Carpeta `\Public`.
    * **Tipus:** *Hard Quota* (Bloqueig estricte d'escriptura en sobrepassar el límit).
    * **Notificació:** Threshold del **90%** amb el següent missatge personalitzat al log d'esdeveniments:
        > "Compte! FoodLogístic t'informa que estàs a punt d'esgotar l'espai compartit.

### 🚫 Filtrat de Fitxers

S'ha configurat un **File Screen** actiu a la carpeta `\Operacions` per mantenir la integritat del servidor:
* **Grups bloquejats:** Fitxers executables (`.exe`, `.msi`) i fitxers d'àudio/vídeo.
* **Mode:** **Actiu** (Impedeix desar el fitxer, no només monitoritza).

---

## ✅ 4. Proves de Verificació (Checklist)

Aquesta taula resumeix els tests realitzats per validar la infraestructura:

| Prova | Resultat Esperat | Estat |
| :--- | :--- | :---: |
| **Accés Usuari Transport** a `\Operacions` | Permès (Accés total) | [ ] |
| **Accés Usuari Administració** a `\Operacions` | Carpeta Invisible (**ABE**) | [ ] |
| **Còpia de `.exe`** a `\Operacions` | Accés Denegat (**FSRM**) | [ ] |
| **Superar 200MB** a `\Public` | Error d'espai insuficient | [ ] |
| **Mapeig Unitat Z:** en Direcció | Apareix automàticament via **GPO** | [ ] |

---

## 📝 Conclusions
La combinació de permisos **NTFS** per a la seguretat local, **SMB** per a la gestió de xarxa i **FSRM** per al control de recursos, garanteix que **FoodLogistic** gaudeixi d'un entorn de dades robust, escalable i sota un control administratiu total.