# 🏢 Anàlisi d'Empreses de Serveis Informàtics

Aquest document detalla les característiques, mides i serveis especialitzats de tres entitats clau en el sector tecnològic local.

---

## 1. D Y D SERVEIS INFORMÀTICS
> **Enfocament:** Suport de proximitat i infraestructura física.

* **Mida:** `Microempresa` 👥
    * Estructures de pocs treballadors que ofereixen un tracte directe i personalitzat.
* **Tipus de serveis:**
    * 🔧 **Manteniment:** Suport tècnic preventiu i reactiu per a parcs informàtics.
    * 💻 **Hardware:** Reparació d'equips i venda de components.
    * 🌐 **Xarxes:** Instal·lació i configuració de xarxes locals (LAN) i connectivitat bàsica.

---

## 2. Serveis Informàtics - Market Software
> **Enfocament:** Capa lògica, gestió de dades i solucions de programari.

* **Mida:** `PIME (Petita Empresa)` 🏢
    * Estructura robusta especialitzada en la gestió de llicències i desenvolupament.
* **Tipus de serveis:**
    * 📊 **Software:** Implantació de programari de gestió (ERP, CRM) i solucions ofimàtiques.
    * 🤝 **Consultoria:** Assessorament sobre eines adaptades al flux de treball.
    * 🛡️ **Seguretat de dades:** Gestió d'antivirus i còpies de seguretat.

---

## 3. Serveis Informàtics Digitalnet
> **Enfocament:** Connectivitat moderna i serveis digitals integrats.

* **Mida:** `PIME` 🚀
    * Requereix personal especialitzat en infraestructures digitals complexes.
* **Tipus de serveis:**
    * 📡 **Xarxes i Telecomunicacions:** Especialistes en WiFi i connectivitat a internet.
    * ☁️ **Serveis al Cloud:** Hosting, correu electrònic corporatiu i solucions al núvol.
    * 🖥️ **Manteniment integral:** Gestió de sistemes (hardware i xarxa).

---

## 📊 Taula Comparativa de Resum

| Empresa | Mida | Especialització Principal |
| :--- | :--- | :--- |
| **D Y D** | `Microempresa` | Suport tècnic i Hardware |
| **Market Software** | `PIME` | Programari de gestió i Consultoria |
| **Digitalnet** | `PIME` | Xarxes, Cloud i Sistemes |

---
*Document creat amb propòsits informatius per a la gestió de proveïdors IT.*

# T01: Coneixent la competència i el sector 🚀

## 📝 Descripció del Projecte
Hem fundat una nova empresa de serveis informàtics a **Mataró** i se'ns presenta la nostra primera gran oportunitat: **FoodLogístic S.A.**, una empresa logística del Polígon de les Hortes. 

Aquest repositori conté l'anàlisi de mercat, l'estructura organitzativa i l'estratègia competitiva necessària per guanyar la licitació d'aquest projecte de renovació tecnològica.

---

### 🏗️ L'Organigrama (Estructura de l'empresa tipus)
A continuació es mostra l'estructura jeràrquica recomanada per a una empresa de serveis IT competitiva al Maresme, generada mitjançant **PlantUML**.

```plantuml
@startuml
header Organigrama Empresa IT Mataró
title Estructura Organitzativa Tipus

node "Direcció General" as DG #Strategy

package "Departament Tècnic" {
    node "Sistemes i Infraestructura" as SIS
    node "Suport / Helpdesk" as HELP
}

package "Departament Comercial" {
    node "Vendes i Màrqueting" as COM
}

package "Administració" {
    node "Recursos Humans i Finances" as ADM
}

DG --> SIS
DG --> HELP
DG --> COM
DG --> ADM

@enduml
````

### 📋 Radiografia de Departaments

* **Sistemes i Infraestructura:** Responsable del disseny, instal·lació i manteniment de servidors, xarxes i seguretat.
* **Suport / Helpdesk:** Atenció directa al client, resolució d'incidències tècniques i manteniment preventiu.
* **Comercial:** Captació de nous clients (com FoodLogístic) i gestió de comptes existents.
* **Administració:** Gestió de facturació, comptabilitat i recursos humans.

---

## 💡 Fase 2: Estratègia

### 🎯 Definició de la Proposta de Valor
Per diferenciar-nos de la competència a Mataró, la nostra estratègia es basa en:

* **Proximitat i Temps de Resposta:** En estar ubicats a la mateixa ciutat, garantim assistència presencial en menys de 2 hores.
* **Especialització Logística:** Adaptem les solucions IT específicament per a entorns de magatzem i distribució alimentària.
* **Monitorització Proactiva:** No esperem que el client ens truqui; detectem l'error abans que s'aturi la producció.

### 👥 Recursos Necessaris
Per donar un servei excel·lent a **FoodLogístic S.A.**, hem definit el següent equip:

1.  **1 Cap de Projecte:** Gestió de la relació amb el client i planificació.
2.  **2 Tècnics de Sistemes:** Per a la renovació física i configuració de xarxes.
3.  **1 Tècnic de Suport (Nivell 1):** Dedicat a les consultes diàries del personal de FoodLogístic.

> [!NOTE]
> Actualment som suficients per a l'arrencada, però preveiem la contractació d'un tècnic addicional si el volum d'incidències de la nova infraestructura supera les 10 setmanals.

---

### 🛠️ Eines Utilitzades
* **PlantUML:** Disseny de l'organigrama mitjançant codi.
* **UMLTree:** Estructuració de jerarquies clares.
* **IA:** Optimització i generació del model estructural.

