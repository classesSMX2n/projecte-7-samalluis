# 💡 Fase 1: Coneixent el terreny i la competència


## 🏢 Anàlisi d'Empreses de Serveis Informàtics

---

### 1. D Y D SERVEIS INFORMÀTICS
> **Enfocament:** Suport de proximitat i infraestructura física.

* **Mida:** `Microempresa` 👥
    * Estructures de pocs treballadors que ofereixen un tracte directe i personalitzat.
* **Tipus de serveis:**
    * 🔧 **Manteniment:** Suport tècnic preventiu i reactiu per a parcs informàtics.
    * 💻 **Hardware:** Reparació d'equips i venda de components.
    * 🌐 **Xarxes:** Instal·lació i configuració de xarxes locals (LAN) i connectivitat bàsica.

---

### 2. Serveis Informàtics - Market Software
> **Enfocament:** Capa lògica, gestió de dades i solucions de programari.

* **Mida:** `PIME (Petita Empresa)` 🏢
    * Estructura robusta especialitzada en la gestió de llicències i desenvolupament.
* **Tipus de serveis:**
    * 📊 **Software:** Implantació de programari de gestió (ERP, CRM) i solucions ofimàtiques.
    * 🤝 **Consultoria:** Assessorament sobre eines adaptades al flux de treball.
    * 🛡️ **Seguretat de dades:** Gestió d'antivirus i còpies de seguretat.

---

### 3. Serveis Informàtics Digitalnet
> **Enfocament:** Connectivitat moderna i serveis digitals integrats.

* **Mida:** `PIME` 🚀
    * Requereix personal especialitzat en infraestructures digitals complexes.
* **Tipus de serveis:**
    * 📡 **Xarxes i Telecomunicacions:** Especialistes en WiFi i connectivitat a internet.
    * ☁️ **Serveis al Cloud:** Hosting, correu electrònic corporatiu i solucions al núvol.
    * 🖥️ **Manteniment integral:** Gestió de sistemes (hardware i xarxa).

---

### 📊 Taula Comparativa de Resum

| Empresa | Mida | Especialització Principal |
| :--- | :--- | :--- |
| **D Y D** | `Microempresa` | Suport tècnic i Hardware |
| **Market Software** | `PIME` | Programari de gestió i Consultoria |
| **Digitalnet** | `PIME` | Xarxes, Cloud i Sistemes |

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

# 💡 Fase 2: Estratègia

### 🎯 Definició de la Proposta de Valor
Ens diferenciarem de la competència establerta a Mataró mitjançant:

* **Proximitat Total:** Som a prop de FoodLogístic, garantint intervencions físiques immediates.
* **Agilitat de Startup:** En ser un equip de 3, el tracte és directe amb els tècnics, sense intermediaris ni esperes burocràtiques.
* **Monitorització 24/7:** Implementació de sistemes de control proactiu per evitar parades en la cadena logística.

### 👥 Recursos Necessaris (Capacitat Actual)
Actualment, la nostra empresa està formada per **3 persones**. Hem decidit repartir-nos les funcions de la següent manera per cobrir el projecte de **FoodLogístic S.A.**:

1.  **Soci 1 (Project Manager & Comercial):** Encarregat de la gestió del client, pressupostos i la part administrativa.
2.  **Soci 2 (Especialista en Sistemes):** Responsable de la renovació de la infraestructura i seguretat de xarxes.
3.  **Soci 3 (Tècnic de Suport/Helpdesk):** Dedicat a la resolució d'incidències diàries i manteniment preventiu.

> [!IMPORTANT]
> **Valoració de personal:** Amb el volum actual de FoodLogístic, **som suficients per arrencar**. No obstant això, si aconseguim un segon client de mida similar, preveiem la contractació externa d'un tècnic de suport nivell 1 per no comprometre la qualitat del servei.

---

### 🛠️ Eines Utilitzades
* **PlantUML:** Generació de diagrames mitjançant codi.
* **UMLTree:** Estructuració jeràrquica.
* **IA:** Suport en l'optimització del model de negoci i codificació del README.
