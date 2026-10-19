# Sistem distribuit de detecție și urmărire a obiectelor cu scanare angulară în timp real

**Proiect bazat pe microcontrolere STM32, FreeRTOS și comunicație CAN**

![Status](https://img.shields.io/badge/Status-În%20dezvoltare-orange)
![Platformă](https://img.shields.io/badge/Platformă-STM32-blue)
![RTOS](https://img.shields.io/badge/RTOS-FreeRTOS-green)
![Comunicație](https://img.shields.io/badge/Comunicație-CAN-purple)
![Limbaj](https://img.shields.io/badge/Limbaj-C%2FC%2B%2B-lightgrey)

## Descrierea proiectului

Proiectul urmărește dezvoltarea unui sistem distribuit în timp real, capabil să detecteze obiecte, să măsoare distanța față de acestea și să transmită informațiile către un nod de monitorizare.

Sistemul va utiliza microcontrolere **STM32**, sistemul de operare în timp real **FreeRTOS** și protocolul de comunicație **CAN** pentru schimbul de date între noduri.

Un senzor de distanță, montat pe un mecanism acționat de un servomotor, va permite scanarea unui sector unghiular. Datele achiziționate vor fi procesate și transmise către un al doilea nod, care va gestiona monitorizarea măsurătorilor și transmiterea comenzilor.

Proiectul pune accent atât pe realizarea unui prototip fizic funcțional, cât și pe analiza performanțelor în timp real ale sistemului.

> **Notă:** Sistemul reprezintă un demonstrator de tip radar, bazat pe măsurarea distanței, și nu un radar radio propriu-zis.

## Obiective

- Realizarea unei aplicații distribuite folosind două noduri cu microcontrolere.
- Implementarea sarcinilor concurente cu ajutorul FreeRTOS.
- Realizarea scanării unghiulare folosind un senzor de distanță și un servomotor.
- Detectarea obiectelor și estimarea distanței și poziției unghiulare a acestora.
- Implementarea comunicației dintre noduri prin protocolul CAN.
- Sincronizarea sarcinilor utilizând cozi de mesaje și/sau semafoare.
- Gestionarea evenimentelor externe și a erorilor de comunicație.
- Evaluarea timpului de răspuns, a latenței, a variației temporale și a fiabilității comunicației.

## Arhitectura sistemului

```text
              ┌──────────────────────────┐
              │     Obiecte externe      │
              └────────────┬─────────────┘
                           │
                           ▼
              ┌──────────────────────────┐
              │ Senzor de distanță       │
              │ + Servomotor de scanare  │
              └────────────┬─────────────┘
                           │
                           ▼
              ┌──────────────────────────┐
              │ Nodul 1 - STM32           │
              │ FreeRTOS                  │
              │                          │
              │ • Achiziția datelor      │
              │ • Controlul servomotorului│
              │ • Detectarea obiectelor  │
              │ • Comunicație CAN        │
              └────────────┬─────────────┘
                           │
                       Magistrală CAN
                           │
                           ▼
              ┌──────────────────────────┐
              │ Nodul 2 - STM32           │
              │ FreeRTOS                  │
              │                          │
              │ • Recepția mesajelor     │
              │ • Procesarea măsurătorilor│
              │ • Monitorizarea sistemului│
              │ • Transmiterea comenzilor│
              └──────────────────────────┘
```

*Arhitectura este preliminară și poate fi ajustată în funcție de componentele hardware selectate.*

## Componente hardware

Configurația finală va fi stabilită în etapa de proiectare. Sunt avute în vedere următoarele componente:

- Două plăci de dezvoltare cu microcontrolere STM32.
- Un senzor de distanță compatibil cu platforma aleasă.
- Un servomotor pentru scanarea unghiulară.
- Transceivere CAN compatibile cu microcontrolerele utilizate.
- Sursă de alimentare și componente electronice auxiliare.
- O interfață pentru afișarea măsurătorilor și a evenimentelor detectate.

## Tehnologii utilizate

| Tehnologie | Rol în proiect |
|---|---|
| C/C++ | Dezvoltarea software-ului embedded |
| STM32 | Platforma hardware principală |
| FreeRTOS | Planificarea și executarea sarcinilor în timp real |
| CAN | Comunicația dintre nodurile sistemului |
| Cozi de mesaje și semafoare | Sincronizarea sarcinilor |
| Git și GitHub | Versionarea codului și documentarea dezvoltării |

## Organizarea sarcinilor în timp real

**Nodul 1 — Achiziție și control**

- Achiziționează datele de la senzorul de distanță.
- Controlează poziția servomotorului.
- Procesează măsurătorile și detectează obiectele.
- Transmite măsurătorile și evenimentele prin CAN.

**Sarcina de comunicație**

- Gestionează transmiterea și recepția mesajelor CAN.
- Transferă datele între sarcinile de comunicație și cele de procesare.
- Utilizează mecanisme de sincronizare pentru accesul sigur la date.
- Gestionează evenimentele și eventualele întreruperi ale comunicației.

**Nodul 2 — Monitorizare și gestionarea comenzilor**

- Recepționează și procesează măsurătorile.
- Furnizează date pentru interfața de monitorizare.
- Transmite comenzi și parametri de configurare.
- Detectează expirarea timpului de așteptare a mesajelor, atunci când este cazul.

Sarcinile vor fi sincronizate folosind mecanismele oferite de FreeRTOS, pentru a preveni condițiile de concurență și inconsistența datelor.

## Analiza performanțelor în timp real

Performanța sistemului va fi evaluată prin intermediul următorilor indicatori:

- **Timpul de răspuns la detecție:** intervalul dintre apariția unui eveniment detectabil și procesarea acestuia.
- **Latența comunicației:** timpul necesar transferului datelor între cele două noduri.
- **Jitter:** variația temporală a execuției sarcinilor sau a livrării mesajelor.
- **Rata de actualizare a scanării:** frecvența cu care sunt actualizate măsurătorile unghiulare.
- **Fiabilitatea comunicației:** comportamentul sistemului la pierderea mesajelor, apariția erorilor pe magistrală sau deconectarea unui nod.

Rezultatele măsurătorilor vor fi documentate și analizate pe parcursul implementării.

## Etapele de dezvoltare

- [ ] Stabilirea configurației hardware finale.
- [ ] Configurarea mediului de dezvoltare pentru STM32.
- [ ] Integrarea FreeRTOS și implementarea sarcinilor de bază.
- [ ] Testarea independentă a achiziției datelor de la senzor.
- [ ] Implementarea controlului servomotorului și a scanării unghiulare.
- [ ] Realizarea și testarea comunicației CAN între noduri.
- [ ] Integrarea procesării măsurătorilor și a detecției obiectelor.
- [ ] Implementarea monitorizării și a comenzilor.
- [ ] Testarea sincronizării și a comportamentului la întreruperea comunicației.
- [ ] Măsurarea și analiza performanțelor în timp real.
- [ ] Pregătirea demonstrației practice și a documentației tehnice.

## Structura repository-ului

Pe măsură ce proiectul se dezvoltă, codul și documentația vor fi organizate astfel:

```text
sistem-detectie-obiecte-stm32/
├── README.md
├── docs/
│   ├── arhitectura_sistemului.md
│   ├── proiectare_hardware.md
│   └── analiza_timp_real.md
├── nod1_achizitie_control/
├── nod2_comunicatie_monitorizare/
├── teste/
└── rezultate/
```

## Stadiul proiectului

**În faza de planificare și selecție a componentelor.**

Arhitectura hardware, implementarea software și rezultatele experimentale vor fi completate pe măsură ce proiectul avansează.

## Context academic

Proiect realizat în cadrul disciplinei dedicate sistemelor embedded în timp real, cu aplicabilitate în achiziția și procesarea datelor, controlul perifericelor, comunicația distribuită și analiza performanțelor temporale.

