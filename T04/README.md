# T04: Implementació de Servidor d’Impressió i Printer Pooling

## 📑 Introducció
En el sector logístic, la continuïtat operativa de la impressió és crítica. Albarans i fulls de transport són vitals per no trencar la cadena de fred i garantir la sortida dels camions. Aquest projecte documenta la configuració d'un **Servidor d'Impressió en Windows Server** amb l'objectiu de centralitzar la gestió i implementar **Printer Pooling** per balancejar la càrrega de treball.

---

## 🏗️ Fase 1: Preparació de l'Entorn i Simulació de Maquinari
En aquesta fase es preparen els dispositius físics (simulats) que rebran les peticions d'impressió.

* **Instal·lació:** S'han configurat dues instàncies de la impressora virtual **PDF24** al servidor.
* **Nomenclatura Corporativa:** Per facilitar la identificació técnica, s'han batejat com:
    * `IMP_MAGATZEM_A`
    * `IMP_MAGATZEM_B`

---

## ⚙️ Fase 2: Instal·lació del Rol i Configuració del Pool
La centralització permet gestionar tots els recursos des d'un únic punt mitjançant la consola de *Print Management*.

### Passos realitzats:
1.  **Instal·lació del Rol:** S'ha activat el rol de **Print and Document Services** mitjançant el *Server Manager*.
2.  **Configuració del Printer Pooling:** * Hem accedit a les propietats de `IMP_MAGATZEM_A`.
    * Dins de la pestanya **Ports**, s'ha habilitat la casella **"Enable printer pooling"**.
    * S'han seleccionat els ports corresponents a les dues impressores (A i B).

> [!IMPORTANT]
> A partir d'aquest moment, el sistema operatiu tracta ambdues impressores com una única unitat lògica. Si un dispositiu està ocupat, el treball es deriva automàticament al següent port lliure.

---

## 🚀 Fase 3: Desplegament Automatitzat (GPO)
Per evitar que els operaris hagin d'instal·lar manualment els dispositius, s'ha utilitzat l'Active Directory.

* **Creació de la GPO:** S'ha generat un objecte de directiva anomenat `GPO_Impressores_Magatzem`.
* **Vinculació:** Mitjançant la funció **"Deploy with Group Policy"** de la consola de gestió d'impressió, s'ha vinculat la impressora a la Unitat Organitzativa (OU) corresponent.
* **Actualització al Client:** En el client Windows 11, s'ha forçat l'aplicació de la directiva amb la següent comanda:

```powershell
gpupdate /force
````

## 🛠️ Fase 4: Prova de Càrrega i Seguretat

Finalment, s'ha validat que la solució compleix amb els requisits d'alta disponibilitat i les polítiques d'ús de l'empresa.

### ⚖️ Simulació de Balanceig (Stress Test)
Per verificar el correcte funcionament del **Printer Pooling**, s'ha realitzat una prova d'estrès:
* **Acció:** Enviament de 10 documents complexos de forma consecutiva a la cua d'impressió compartida.
* **Resultat:** El servidor ha distribuït dinàmicament els treballs entre `IMP_MAGATZEM_A` i `IMP_MAGATZEM_B`.
* **Conclusió:** S'ha evitat el col·lapse de la cua única i es garanteix que, si una impressora s'atura, l'altra continua processant la feina.

### 🔐 Restriccions Operatives i Seguretat
S'han aplicat configuracions avançades per optimitzar el manteniment i el control del consum:

| Configuració | Detall de la Implementació |
| :--- | :--- |
| **Horari de Disponibilitat** | La impressora només accepta treballs de **06:00 a 22:00**. |
| **Gestió de Cua** | Els documents enviats fora d'hores es mantenen retinguts fins a l'inici del torn. |
| **Prioritats** | S'ha definit la prioritat de processament segons la jerarquia d'usuaris del domini. |

---

## 📂 Material de Suport
* 📘 **0224 SOX. Material UD8: AA3** – [Accés al Moodle de l’assignatura]
* 🛠️ **Guia Tècnica** – Instal·lació i configuració de controladors PDF24.

---
> **Informe generat per:** [El Teu Nom/Usuari]  
> **Data:** 2026  
> **Estat:** ✅ Finalitzat i verificat