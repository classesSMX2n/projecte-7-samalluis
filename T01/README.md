
# T01: Coneixent la competència i el sector 🚀

## 📝 Descripció del Projecte
Hem fundat una nova empresa de serveis informàtics a **Mataró** i se'ns presenta la nostra primera gran oportunitat: **FoodLogístic S.A.**, una empresa logística del Polígon de les Hortes. 

Aquest repositori conté l'anàlisi de mercat, l'estructura organitzativa i l'estratègia competitiva necessària per guanyar la licitació d'aquest projecte de renovació tecnològica.

---

## 🔍 Fase 1: Coneixent el terreny i la competència

### 📊 Recerca de Mercat
Hem identificat tres empreses clau que operen a la zona del Maresme per entendre el nostre entorn competitiu:

| Empresa | Ubicació | Mida | Serveis Principals |
| :--- | :--- | :--- | :--- |
| **Empresa A** | Mataró | PIME | Manteniment IT, Seguretat i Xarxes |
| **Empresa B** | Vilassar de Mar | Microempresa | Desenvolupament Web i Software a mida |
| **Empresa C** | Mataró | PIME | Consultoria Cloud i Helpdesk 24/7 |

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
