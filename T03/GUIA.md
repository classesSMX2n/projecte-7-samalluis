# T03: Servidor de fitxers – Informe Tècnic

**Informe Tècnic – T03: Servidor de Fitxers**  
**Domini:** FoodLogistic.test  
**Nom del servidor:** FL07
**Assignatura:** Sistemes Operatius en Xarxa  
**Nom Alumne:** Lluís García Martínez

---

## 1. Estructura d’OUs i Grups de Seguretat (Active Directory)

### Objectiu
Preparar l’Active Directory amb una estructura lògica i escalable abans de desplegar el servidor de fitxers, seguint el principi de **menor privilegi** (*least privilege*) i facilitant l’aplicació de GPOs per departament.

### Raonament
Una bona estructura d’Unitats Organitzatives (OUs) permet:
- Aplicar **GPOs** de forma granular (una per departament si cal).
- **Delegar** l’administració d’usuaris a caps de departament sense donar permisos de domini.
- Mantenir l’ordre i la traçabilitat en entorns que creixen.

### Estructura d’OUs creada

```
foodlogistic.test
├── OU=FoodLogistic
│   ├── OU=Grups
│   │   ├── Group=Administracio
│   │   ├── Group=Transport
│   │   └── Group=Direccio
│   ├── OU=Usuaris
```

![alt text](<picsf2/Captura de pantalla 2026-04-14 163138.png>)

### Grups de Seguretat creats (Global Security Groups)

| Nom del grup | Tipus | Ubicació | Finalitat |
|-------------|-------|----------|-----------|
| `Administracio` | Global Security | OU=Grups | Gestió de factures i albarans |
| `Transport` | Global Security | OU=Grups | Xofers i caps de flota |
| `Direccio` | Global Security | OU=Grups | Gerència i accés a documentació confidencial |

> **Nota:** Aquests grups són **Global Security Groups**. S’utilitzen tant per assignar permisos NTFS/SMB com per filtrar l’aplicació de GPOs.

---

## 2. Resum de Configuració

| Carpeta | Camí físic | Camí UNC | Grups amb accés | Mètode de creació | Permisos efectius (xarxa) | Observacions |
|---------|-----------|----------|----------------|-------------------|---------------------------|--------------|
| **Public** | `C:\Public` | `\\FL07\Public` | Tothom (Everyone) | Explorador de fitxers | **Lectura** | Sense ABE |
| **Operacions** | `C:\Operacions` | `\\FL07\Operacions` | `Transport` | Server Manager (FSSM) | **Modificació** + ABE | Només visible per Transport |
| **Direccio** | `C:\Direccio` | `\\FL07\Direccio` | `Direccio` | **PowerShell Avançat (Opció D)** | **Control Total** + ABE | Unitat **C:** via GPO |

**Servidor:** FL07.foodlogistic.test (Windows Server 2022)

![alt text](<picsf2/Captura de pantalla 2026-04-14 163256.png>)

---

## 3. Implementació de Recursos Compartits

### A. Carpeta **Public** (Mètode: Explorador de fitxers)

#### Objectiu
Crear una carpeta accessible per a tots els usuaris de l’empresa utilitzant **exclusivament** l’Explorador de Windows, tal com exigeix l’enunciat.

#### Raonament
Aquest apartat demostra el domini de la via gràfica bàsica. A més, l’enunciat proposa una combinació intencionada de permisos SMB i NTFS perquè observem com es calculen els **permisos efectius**.

#### Passos detallats

1. **Creació de la carpeta física**
   - Ens connectem al servidor `FL07` amb un compte d’**Administrador de Domini**.
   - Obrim l’**Explorador de fitxers** i anem a la unitat `C:\`.
   - Clic dret → **Nou** → **Carpeta** → Li posem el nom **`Public`**.
   - Ruta resultant: `C:\Public`

2. **Configuració dels permisos de compartició (SMB)**
   - Clic dret sobre `C:\Public` → **Propietats** → Pestanya **Compartir** → **Compartir...**
   - A la llista desplegable, seleccionem **Everyone** i li assignem el nivell **Lectura**.
   - Clic a **Compartir** i després **Fet**.

![alt text](<picsf2/Captura de pantalla 2026-05-22 173742.png>)

3. **Configuració dels permisos NTFS**
   - A la mateixa finestra de **Propietats**, anem a la pestanya **Seguretat**.
   - Clic a **Edita** → **Afegir**.
   - Escrivim `Everyone` i li donem el permís **Modificació** (Modify).
   - Assegurem que el grup **Administradors** tingui **Control total**.
   - Clic a **Aplica** i **D’acord**.

![alt text](<picsf2/Captura de pantalla 2026-05-22 173934.png>)

#### Explicació tècnica: Càlcul del permís efectiu

Quan un usuari accedeix a una carpeta compartida per xarxa, Windows aplica **dues capes de seguretat**:
1. **Permisos SMB** (de la compartició).
2. **Permisos NTFS** (del sistema de fitxers).

La regla és clara: el permís efectiu és **el més restrictiu** de tots dos.

| Capa | Permís assignat |
|------|----------------|
| SMB (Share) | Everyone → **Lectura** |
| NTFS | Everyone → **Modificació** |
| **Permís efectiu final** | **Lectura** |

Això vol dir que, tot i que NTFS permetria escriure, la capa SMB ho restringeix a només lectura. En entorns professionals, sovint es posa **Canvi (Change)** a SMB i es detallen els permisos a nivell NTFS.

---

### B. Carpeta **Operacions** (Mètode: Server Manager – FSSM)

#### Objectiu
Crear el recurs compartit `Operacions` des de la consola **Server Manager**, activant **Access-Based Enumeration (ABE)** i restringint l’accés exclusivament al grup `Transport`.

#### Raonament
El Server Manager ofereix una interfície més professional i completa que l’Explorador. Permet crear el share i la carpeta alhora, i configurar ABE directament durant el procés.

#### Passos detallats

1. Obrim **Server Manager** → **File and Storage Services** → **Shares**.
2. Al panell de la dreta, clic a **TASKS** → **New Share...**
3. Seleccionem el perfil **SMB Share – Quick**.
4. A **Share location**, triem el servidor i especifiquem la ruta personalitzada:
   ```
   C:\Operacions
   ```
   > Si la carpeta no existeix, l’assistent la crea automàticament.

5. A **Share name**, posem: `Operacions`.
6. A la pantalla **Other settings**, marquem la casella clau:
   - ☑ **Enable access-based enumeration**
   - Això farà que els usuaris només vegin aquesta carpeta si tenen permisos explícits sobre ella.

![alt text](<picsf2/Captura de pantalla 2026-05-22 174659.png>)

![alt text](<picsf2/Captura de pantalla 2026-05-22 174620.png>)

7. A la pantalla **Permissions**, eliminem els permisos per defecte i afegim **només** el grup `Transport` amb permisos de **Modificació** (Modify).
   - També assegurem que **Administradors** mantingui **Control total**.

![alt text](<picsf2/Captura de pantalla 2026-05-22 174948.png>)

8. Revisem el resum i cliquem **Create**.


#### Explicació tècnica
L’**Access-Based Enumeration (ABE)** és una funcionalitat del protocol SMB que filtra la llista de fitxers i carpetes abans d’enviar-la al client. Si l’usuari no té permís de lectura sobre un element, aquest simplement **no apareix** en l’explorador de xarxa. Això millora la seguretat i evita confusions.

---

### D. Carpeta **Direccio** (Mètode: PowerShell Avançat – Opció D)

#### Objectiu
Crear una carpeta confidencial només accessible pel grup `Direccio`, utilitzant **PowerShell** amb ABE, i configurar una unitat de xarxa automàtica mitjançant **GPO**.

#### Raonament
Aquest és el mètode més avançat i professional. Permet automatitzar la creació, aplicar configuracions complexes (com ABE via cmdlet) i deixa tot documentat en script per si cal replicar l’entorn.

#### Passos detallats i codi

**Paso 1: Crear la carpeta física**
```powershell
New-Item -Path "C:\Direccio" -ItemType Directory -Force
```

**Paso 2: Crear el recurs compartit amb ABE activat**
> **Atenció:** El valor correcte és `AccessBased` (amb doble 's').

```powershell
New-SmbShare -Name "Direccio" `
             -Path "C:\Direccio" `
             -FullAccess "Direccio" `
             -FolderEnumerationMode AccessBased `
             -CachingMode None `
             -Description "Carpeta confidencial solo para Direccion"
```

**Paso 3: Reforçar permisos NTFS**
```powershell
# Eliminar herència i netejar permisos heretats
icacls "C:\Direccio" /inheritance:r

# Assignar Control Total al grup Direccio (OI=Object Inherit, CI=Container Inherit)
icacls "C:\Direccio" /grant "Direccio:(OI)(CI)F"

# Assignar Control Total als Administradors
icacls "C:\Direccio" /grant "Administradores:(OI)(CI)F"
```

**Paso 4: Verificació del share creat**
```powershell
Get-SmbShare -Name "Direccio" | Format-List
```
> Resultat esperat: `FolderEnumerationMode : AccessBased`

![Verificació del share Direccio via PowerShell](<pics/Captura de pantalla 2026-04-21 161936.png>)

#### Configuració de la GPO per mapar la unitat C:

Perquè els usuaris de Direcció tinguin la carpeta accessible automàticament com a unitat **Z:**, creem una GPO filtrada.

1. Obrim **Group Policy Management** (`gpmc.msc`).
2. Creem una nova GPO anomenada: `Map Drive Direccio - C:`.
3. L’enllacem a l’OU `Usuaris\Direccio`.
4. Al filtre de seguretat, eliminem **Authenticated Users** i afegim **només** el grup `Direccio`.

   ![Filtre de seguretat de la GPO](<pics/Captura de pantalla 2026-04-21 161951.png>)

5. Editem la GPO i anem a:
   ```
   User Configuration → Preferences → Windows Settings → Drive Maps
   ```
6. Clic dret → **New** → **Mapped Drive**:
   - **Action:** Create
   - **Location:** `\\FL07\Direccio`
   - **Reconnect:** ☑ Enabled
   - **Label as:** Direccio
   - **Drive Letter:** C:
   - **Hide/Show this drive:** Show this drive

   ![Configuració del Drive Map a la GPO](<pics/Captura de pantalla 2026-04-21 162547.png>)

7. Tanquem l’editor i forcem l’actualització de la política als clients amb:
   ```cmd
   gpupdate /force
   ```

   ![Resultat de la GPO aplicada al client](<pics/Captura de pantalla 2026-04-21 163107.png>)

---

## 4. Control d’Emmagatzematge (Quotas i FSRM)

### 4.1 Quota NTFS (per volum – C:)

#### Objectiu
Limitar l’espai que pot ocupar cada usuari nou al volum de dades.

#### Passos
1. Obrim l’**Explorador de fitxers** → Clic dret sobre la unitat `C:` → **Propietats**.
2. Pestanya **Quota** → Clic a **Show Quota Settings**.
3. Activem ☑ **Enable quota management**.
4. Marquem ☑ **Deny disk space to users exceeding quota limit**.
5. Seleccionem **Limit disk space to**: `500 MB`.
6. Establim el llindar d’avís a `85%`.
7. Clic a **OK**.

![Configuració de la quota NTFS al volum C:](<pics/Captura de pantalla 2026-04-21 165423.png>)

---

### 4.2 Quota FSRM a la carpeta Public (per carpeta)

#### Objectiu
Aplicar una quota estricta (**Hard Quota**) de 200 MB específicament a la carpeta `C:\Public`, amb un avís personalitzat quan s’arribi al 90%.

#### Passos detallats

1. Instal·lem el rol **File Server Resource Manager** (si no ho havíem fet ja):
   - Server Manager → Manage → Add Roles and Features → File and Storage Services → File Server Resource Manager.

2. Obrim el FSRM:
   - `Windows + R` → `fsrm.msc` → **Enter**.

3. Al panell esquerre, despleguem **Quota Management** → **Quotas**.
4. Clic dret → **Create Quota...**
5. A **Quota path**, introduïm:
   ```
   C:\Public
   ```
6. Seleccionem **Define custom quota properties** → Clic a **Custom Properties...**

7. A la finestra de propietats:
   - **Space limit:** `200` MB
   - Seleccionem **Hard quota** (bloca l’escriptura quan s’arriba al límit).

8. Afegim un llindar d’avís (Threshold):
   - Clic a **Add** → **Generate notifications when usage reaches:** `90`%.
   - A la pestanya **E-mail Message** (o **Event Log** si no tenim SMTP configurat), escrivim el missatge:
     > "Compte! FoodLogístic t'informa que estàs a punt d'esgotar l'espai compartit."

9. Clic a **OK** → **Create**.

![Configuració de la quota FSRM](<pics/Captura de pantalla 2026-04-21 165053.png>)

![Quota FSRM aplicada a C:\Public](<pics/Captura de pantalla 2026-04-21 165047.png>)

---

### 4.3 Filtrat de fitxers (File Screen) a Operacions

#### Objectiu
Impedir que els usuaris del grup Transport puguin guardar fitxers executables, d’àudio, vídeo i altres extensions no autoritzades dins `C:\Operacions`.

#### Raonament
FSRM permet crear **pantalles de fitxers** (*File Screens*) que actuen a nivell de carpeta. A diferència del filtratge per extensió bàsic, FSRM pot analitzar la signatura interna del fitxer, de manera que canviar l’extensió no enganya el sistema.

#### Passos detallats

**Paso 1: Crear un Grup de Fitxers personalitzat**

1. Dins el **FSRM**, despleguem **File Screening Management** → **File Groups**.
2. Clic dret → **Create File Group...**
3. **File group name:** `Extensions Prohibides Extra`
4. A **Files to include**, afegim una a una:
   - `*.bat`
   - `*.cmd`
   - `*.mkv`
   - `*.mov`
5. Clic a **OK**.

**Paso 2: Crear el File Screen**

1. Anem a **File Screening Management** → **File Screens**.
2. Clic dret → **Create File Screen...**
3. **File screen path:** `C:\Operacions`
4. Seleccionem **Define custom file screen properties** → **Custom Properties...**
5. **Screening type:** seleccionem obligatòriament **Active screening** (bloca realment el fitxer; el *Passive* només registra).
6. A **File groups**, marquem:
   - ☑ **Executable Files**
   - ☑ **Audio and Video Files**
   - ☑ **Extensions Prohibides Extra**

   ![Creació del File Screen](<pics/Captura de pantalla 2026-04-21 165938.png>)

7. (Opcional) A la pestanya **Actions**, podem afegir un missatge al registre d’esdeveniments.
8. Clic a **OK** → **Create**.

   ![Propietats del File Screen](<pics/Captura de pantalla 2026-04-21 170321.png>)

   ![File Screen actiu a C:\Operacions](<pics/Captura de pantalla 2026-04-21 170201.png>)

#### Explicació tècnica
El filtratge actiu de FSRM analitza la **signatura interna** del fitxer (els seus *magic numbers*), no només l’extensió del nom. Per tant, si un usuari canvia `virus.exe` a `virus.txt`, el sistema **continuarà bloquejant-lo** perquè detecta que és un executable.

---

## 5. Proves de Funcionament (des de client Windows 10/11)

Per verificar que tota la infraestructura funciona correctament, hem realitzat les següents comprovacions des d’un client unit al domini:

### 5.1 Verificació d’accés i visibilitat per perfil d’usuari

| Tipus d’usuari | Carpetes visibles | Unitat C: | Pot escriure? |
|----------------|-------------------|-----------|---------------|
| **Transport** | Veu `Public` i `Operacions` | No apareix | Sí a `Operacions`, només lectura a `Public` |
| **Direccio** | Veu `Public`, `Operacions` i `Direccio` | Sí (C:) | Sí a `Direccio`, lectura a `Public` |
| **Administracio** | Veu `Public` i `Operacions` (no veu `Direccio` per ABE) | No apareix | Només lectura a `Public` |

- ![alt text](<pics/Captura de pantalla 2026-04-21 172123.png>)
- ![alt text](<pics/Captura de pantalla 2026-04-21 191136.png>)
- ![alt text](<pics/Captura de pantalla 2026-04-21 191146.png>)

> **Observació:** La carpeta `Direccio` no apareix als usuaris que no en formen part gràcies a l’**ABE** configurat tant al Server Manager com a PowerShell.

### 5.2 Prova de filtratge de fitxers (Operacions)

- **Intent 1:** Copiar `notepad.exe` a `\\FL11\Operacions` → **Bloquejat per FSRM**.
- **Intent 2:** Renombrar `notepad.exe` a `notepad.txt` i copiar-lo → **Continua bloquejat**.  
  Això demostra que el filtratge actiu no es deixa enganyar pel canvi d’extensió.


![alt text](pics/image5.png)
![alt text](pics/image6.png)

### 5.3 Prova de quota (Public)

- Omplim la carpeta `Public` fins a 180 MB (90% de 200 MB).
- Resultat: Apareix l’avís personalitzat configurat al FSRM.
- Intentem superar els 200 MB: el sistema denega l’escriptura amb error d’**espai insuficient** (comportament de *Hard Quota*).

![alt text](pics/image7.png)

![alt text](pics/image8.png)
---

## 6. Conclusions i Millores Aplicades

S’ha aconseguit implementar una infraestructura de fitxers **segura, organitzada i controlada** per a FoodLogistic, complint tots els requisits de l’enunciat:

- **Tres vies d’administració demostrades:** Explorador de fitxers (GUI bàsica), Server Manager (GUI avançada) i PowerShell (línia de comandes/scripting).
- **Seguretat per capes:** combinació òptima de permisos SMB + NTFS + ABE + GPOs filtrades.
- **Control d’espai:** quotes NTFS per volum (500 MB/usuari) i quotes FSRM per carpeta (200 MB a Public amb avís al 90%).
- **Prevenció de contingut no desitjat:** File Screen actiu a `Operacions` bloquejant executables i multimèdia.

**Recomanacions futures:**
- Implantar **DFS Namespace** per unificar els camins UNC i facilitar la migració futura de servidors.
- Implementar **Dynamic Access Control (DAC)** per a una gestió més granular basada en atributs d’usuari.
- Configurar **Shadow Copies** (còpies d’ombra) per permetre la recuperació de fitxers per part dels usuaris sense intervenció de l’administrador.



