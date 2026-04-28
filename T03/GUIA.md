# Guia de Configuració: Servei de Fitxers (FoodLogistic)

Aquest document detalla el procediment tècnic per a la implementació i gestió d'un servidor de fitxers professional sota l'entorn de domini **FoodLogistic**.

---

## 1. Preparació de l'Active Directory (AD)

Abans de la configuració del servidor, cal establir una estructura de grups robusta.

### Creació d'Unitats Organitzatives (OU)
1. Obre **Active Directory Users and Computers**.
2. Crea una OU principal anomenada `FoodLogistic`.
3. Dins de `FoodLogistic`, crea dues sub-OUs:
   - `Grups`
   - `Usuaris`

### Creació de Grups de Seguretat
Dins de l'OU `Grups`, crea els següents grups de seguretat d'àmbit **Global**:
- **Administracio**
- **Transport**
- **Direccio**

> **Justificació:** Aquesta estructura permet delegar permisos de forma escalable i aplicar GPOs específiques per departament en el futur.

---

## 2. Implementació de Recursos Compartits

### A. Carpeta Public (Mètode: Explorador de fitxers)
* **Ruta local:** `D:\Public`
* **Permisos SMB (Sharing):** - Comparteix com a "Public".
    - `Everyone`: Només **Read**.
* **Permisos NTFS (Security):** - `Everyone` (o `Domain Users`): Permís de **Modify**.
* **Nota tècnica:** El permís efectiu és el més restrictiu. Tot i que a NTFS poden modificar, via xarxa (SMB) només podran llegir.

### B. Carpeta Operacions (Mètode: Server Manager)
1. Ves a **Server Manager** > **File and Storage Services** > **Shares**.
2. Selecciona **Tasks** > **New Share** > **SMB Share - Quick**.
3. **Path:** `D:\Operacions`.
4. **Settings:** Marca la casella **Enable access-based enumeration (ABE)**.
5. **Permissions:** Clica a *Customize permissions*.
    - Desactiva l'herència (*Disable inheritance*).
    - Elimina els usuaris ordinaris.
    - Afegeix el grup **Transport** amb permís de **Modify**.

### C. Carpeta Confidencial (Mètode: PowerShell Avançat)
Execució de script per a l'automatització i precisió en la configuració:

```powershell
# 1. Crear la carpeta
New-Item -Path "D:\Direccio" -ItemType Directory

# 2. Compartir amb Access-Based Enumeration (ABE) i permisos per al grup Direccio
New-SmbShare -Name "Direccio" -Path "D:\Direccio" -FolderEnumerationMode AccessBased -FullAccess "foodlogistic\Direccio"

# 3. Configurar Permisos NTFS (evitant que altres usuaris la vegin)
$Acl = Get-Acl "D:\Direccio"
$Ar = New-Object System.Security.AccessControl.FileSystemAccessRule("foodlogistic\Direccio", "Modify", "ContainerInherit,ObjectInherit", "None", "Allow")
$Acl.SetAccessRule($Ar)
Set-Acl "D:\Direccio" $Acl