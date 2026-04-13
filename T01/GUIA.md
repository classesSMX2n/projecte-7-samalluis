# T01: Coneixent la competència i el sector

## Grup de treball
- Albert Teruel
- Joel Dominguez
- Lluis García

---

## Fase 1: Coneixent el terreny i la competència

### 1.1 Recerca de mercat

Hem investigat el mercat local de Mataró per saber qui més podria presentar una oferta a FoodLogístic S.A.  
Hem seleccionat aquestes tres empreses amb seu física a la ciutat:

| Empresa | Mida | Serveis principals | Pàgina Web |
|----------|------|--------------------|-------------|
| Main Memory | PIME | Gestió d'infraestructures, manteniment preventiu i seguretat. | [main.cat](https://www.mainmemory.es/ ) |
| Ilimit | PIME | Especialistes en Cloud, ciberseguretat i servidors gestionats. | [ilimit.com](https://ilimit.com/) |
| Infored | Micro / PIME | Manteniment informàtic local, xarxes i venda de hardware. | [infored.es](https://infored.es/ ) |

---

### 1.2 L’organigrama

Hem definit l'estructura jeràrquica d'una empresa de serveis informàtics mitjana de Mataró (com podria ser Main o Ilimit ja que aquestas dades no son publiques).

<img width="906" height="453" alt="image" src="https://github.com/user-attachments/assets/4f40123a-0d66-465a-abe4-43aa5c8e95fd" />

#### Fragment de codi UML
```
@startuml
skinparam monochrome true
skinparam packageStyle rectangle
skinparam shadowing false

node "Direcció General\n(Estratègia i Decisions)" as CEO

node "Administració i RH\n(Facturació i Talent)" as Admin
node "Comercial i Màrqueting\n(Vendes i Clients)" as Comercial
node "Direcció Tècnica (CTO)\n(Operacions i Tecnologia)" as CTO

node "Àrea d'Infraestructura\ni Sistemes" as Sist
node "Àrea de Suport\ni Helpdesk" as Suport
node "Àrea de Ciberseguretat\ni Cloud" as Security

node "Administradors de Xarxes" as Xarxes
node "Tècnics L1/L2\n(Atenció Directa)" as Tècnics
node "Tècnics de Camp\n(Desplaçaments)" as Camp

CEO --> Admin
CEO --> Comercial
CEO --> CTO

CTO --> Sist
CTO --> Suport
CTO --> Security

Sist --> Xarxes
Suport --> Tècnics
Suport --> Camp
@enduml
```

---

### 1.3 Radiografia de departaments

Aquesta és la justificació de les funcions per a l'empresa tipus:

Direcció Tècnica (CTO): És el cervell de l'empresa. Decideix quines tecnologies s'aplicaran a FoodLogístic.

Àrea d'Infraestructura i Sistemes: S’encarreguen dels servidors i que la xarxa no caigui. És el departament on treballaria en Joel Dominguez.

Àrea de Suport i Helpdesk: És la cara visible per al client. Resolvèn els dubtes diaris. Aquí és on treballaria en Lluis García.

Àrea Comercial i Administració: S’encarreguen dels contractes, les factures i de que el client estigui content. És l'àrea gestionada per l'Albert Teruel.

Àrea de Ciberseguretat: Un departament clau avui dia per protegir les dades de logística d'atacs externs.

---

## Fase 2: Estratègia

### 2.1 Definició de l’estratègia

La nostra proposta guanyadora contra empreses més grans com Main o Ilimit serà el Suport Tècnic de KM.0.

Justificació:  
FoodLogístic està al Camí Ral (Polígon de les Hortes) i nosaltres som de Mataró.  
Mentre que una empresa gran pot trigar hores a enviar un tècnic, nosaltres garantim que estarem a les seves oficines en 15 minuts davant qualsevol aturada crítica del sistema de càrrega de camions.

### 2.2 Recursos necessaris

Per cobrir les necessitats de FoodLogístic S.A., hem definit el següent equip:

- Cap de Projecte i Administració (Albert Teruel): S'encarregarà de la part legal, la compra de material i de parlar amb els directius del client.

- Enginyer de Sistemes i Xarxes (Joel Dominguez): Serà el responsable de dissenyar la nova infraestructura i configurar els servidors i la seguretat.

- Tècnic de Suport i Helpdesk (Lluis García): S'encarregarà d'atendre els treballadors de la nau i resoldre els problemes del dia a dia amb els ordinadors i les terminals.

#### Som suficients o ens cal més personal?

La nostra conclusió és que per al manteniment diari som suficients, però per a la fase de renovació a fons de la infraestructura (que és un volum de feina molt gran i físic), necessitarem:

Contractació externa temporal: Necessitarem contractar una empresa de cablejat o un parell de tècnics autònoms durant les dues primeres setmanes.

Raonament: Una empresa de logística al Polígon de les Hortes té una nau gran. Nosaltres tres ens hem de centrar en la configuració i la gestió; si ens posem a tirar cable o muntar racks físicament, el projecte anirà massa lent i no podrem atendre les urgències del client.

---
