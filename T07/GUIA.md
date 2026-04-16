# Proposta Tècnica i Comercial: Modernització Digital FoodLogistic

## 1. Resum i Objectius Estratègics

FoodLogistic requereix la modernització de la seva infraestructura (35 usuaris) per resoldre la inseguretat i ineficiència del hosting actual. Els objectius clau són:

Escalabilitat total: garantir operativa sense bloquejos per espai en els propers 10 anys.
Ciberseguretat activa: blindar el domini per evitar suplantacions d’identitat.
Entorn col·laboratiu: integració de xat, videoconferències i edició en temps real.

## 2. Anàlisi del Mercat

Microsoft 365: referent en control documental i emmagatzematge massiu.
Google Workspace: agilitat i treball nadiu en navegador sense dependència local.
Zoho Workplace: suite integrada amb una ràtio de cost altament competitiva.
Lark Suite: plataforma "tot en un" emergent amb forta gestió de projectes.

## 3. Comparativa Tècnica de Solucions

| Paràmetre tècnic | Microsoft 365 Basic | Google Workspace Starter | Zoho Workplace Standard |
|---|---:|---:|---:|
| Emmagatzematge | 1.074 GB | 30 GB | 40 GB |
| Cost llicència | 5,60 € | 5,75 € | 3,00 € |
| Seguretat | Entra ID / AES-256 | Zero Trust / TLS | 2FA / xifrat |

## 4. Justificació de la Decisió i Descarts de Competidors

Es recomana Microsoft 365 Business Basic per la seva superioritat tècnica absoluta:

Descart de Zoho: ineficiència d’espai i alta fricció en formats Excel que trenquen macros logístiques.
Descart de Google: la pitjor ràtio d’inversió; Microsoft entrega 35,8 vegades més espai per un preu menor.
Capacitat inigualable: 1.074 GB totals que eliminen qualsevol restricció d’espai a llarg termini.
Estandardització: interoperabilitat total amb el sector logístic gràcies a l’ús natiu d’Excel i Word.

## 5. Arquitectura de Seguretat Microsoft

La solució inclou un blindatge professional superior a la competència:

Entra ID: gestió d’identitat professional per al control total d’accessos.
Cifratge AES-256: protecció de dades en repòs i trànsit per a tota la logística.
Higiene de domini: configuració SPF i DKIM per garantir l’entrega de correu.
Protocol DMARC: bloqueig de suplantacions d’identitat, estàndard 2026.

## 6. Pressupost Global Consolidat (TCO)

| Concepte | Any 1 (Activació + Llicències) | Any 2 i posteriors (Recurrent) |
|---|---:|---:|
| Llicències (35 usuaris) | 2.352,00 € | 2.352,00 € |
| Implementació i migració | 450,00 € | 0,00 € |
| TOTAL | 2.802,00 € | 2.352,00 € / any |

Nota: inversió blindada contra la inflació de dades gràcies a la capacitat d’1 TB per usuari.

## 7. Metodologia de Migració

Es farà servir el Servei de Migració d’Exchange (EAC) mitjançant el protocol IMAP per garantir la persistència de l’historial i les carpetes.

Fases: auditoria, sincronització en segon pla i validació final d’integritat.
Seguretat DNS: implementació de SPF, DKIM i DMARC inclosa en el desplegament.
Limitació: el protocol IMAP no migra calendaris o contactes locals; aquests requeriran importació manual.
