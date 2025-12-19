# SYSTÈME DE SURVEILLANCE MULTIPARAMÉTRIQUE EN SANTÉ CONNECTÉE
## Cahier des Charges et Document de Projet

**Auteur :** Christophe CROISANT (NTIConseil) 
**Date :** Décembre 2025  
**Version :** 1.0  
**Statut :** Document de Référence  

---

## TABLE DES MATIÈRES

1. [Contexte et Objectifs](#1-contexte-et-objectifs)
2. [Spécifications Fonctionnelles](#2-spécifications-fonctionnelles)
3. [Spécifications Techniques](#3-spécifications-techniques)
4. [Architecture du Système](#4-architecture-du-système)
5. [Calendrier et Jalons](#5-calendrier-et-jalons)
6. [Plan d'Acquisition](#6-plan-dacquisition)
7. [Critères d'Acceptation](#7-critères-dacceptation)
8. [Risques et Mitigations](#8-risques-et-mitigations)
9. [Ressources et Budget](#9-ressources-et-budget)

---

## 1. CONTEXTE ET OBJECTIFS

### 1.1 Problématique

La surveillance continue des paramètres vitaux est essentielle en médecine préventive et pour la prise en charge de patients atteints de pathologies chroniques. Les systèmes de monitoring classiques sont souvent chers, peu portables et difficiles d'accès dans les régions rurales ou à ressources limitées.

**Notre objectif :** Concevoir un prototype de système portable et peu coûteux, capable de mesurer plusieurs paramètres vitaux simultanément et de les transmettre sans fil pour analyse et alerte.

### 1.2 Objectifs Pédagogiques

Ce projet vise à développer des compétences multidisciplinaires :

| Domaine | Objectifs Spécifiques |
|---------|----------------------|
| **Électronique Embarquée** | Interfaçage capteurs, gestion I2C, Arduino |
| **Théorie du Signal** | Filtrage numérique, traitement des signaux biomédicaux |
| **Communication** | Protocoles sans fil (Bluetooth), transmission de données |
| **Analyse de Données** | Stockage, visualisation, tendances |
| **IA/Machine Learning** | Classification d'anomalies (version avancée) |
| **Méthodologie Projet** | Cahier des charges, planification, tests |

### 1.3 Résultats Attendus

- ✅ Prototype fonctionnel mesurant 3 paramètres vitaux
- ✅ Application mobile visualisant les données en temps réel
- ✅ Documentation complète et code source commenté
- ✅ Rapport d'analyse et tests de validation
- ✅ Démonstration pratique du système

---

## 2. SPÉCIFICATIONS FONCTIONNELLES

### 2.1 Besoins Utilisateur

#### 2.1.1 Patient / Porteur du Dispositif

| Besoin | Description |
|--------|-------------|
| **Mesure autonome** | Utiliser le système sans technicien à proximité |
| **Confort** | Appareil portable, non invasif, léger |
| **Feedback immédiat** | Voir ses mesures en temps réel |
| **Sécurité** | Alertes en cas de valeurs anormales |
| **Facilité d'usage** | Interface intuitive, peu de formation |

#### 2.1.2 Professionnel de Santé

| Besoin | Description |
|--------|-------------|
| **Surveillance à distance** | Consulter les patients depuis son cabinet |
| **Historique** | Accéder aux données passées et tendances |
| **Alertes critiques** | Notification immédiate des anomalies |
| **Décisions éclairées** | Données précises et fiables |
| **Conformité** | Système conforme aux normes médicales |

### 2.2 Fonctionnalités Principales

#### Phase 1 (Semaines 1-4) : MVP (Minimum Viable Product)

**F1.1 - Acquisition de Données Biomédicales**
- Mesure de la fréquence cardiaque (BPM) via MAX30102
- Mesure de la saturation en oxygène (SpO2 %) via MAX30102
- Mesure de la température corporelle (°C) via MLX90614
- Fréquence d'acquisition : 1 Hz (1 mesure/seconde)
- Précision : ±2% pour SpO2, ±1 BPM, ±0.5°C

**F1.2 - Affichage Local**
- Écran LCD I2C 16×2 affichant les 3 paramètres en temps réel
- Format : BPM: 72 | SpO2: 98% | Temp: 37.2°C
- Mise à jour chaque seconde

**F1.3 - Traitement du Signal Basique**
- Moyenne mobile sur 5 dernières mesures pour lissage
- Détection simple des anomalies :
  - SpO2 < 90% → ALERTE BASSE
  - BPM > 120 ou < 60 → ALERTE CARDIAQUE
  - Température > 38°C → ALERTE FIÈVRE

**F1.4 - Communication Série**
- Envoi des données brutes vers ordinateur via USB
- Format de trames : `BPM,SpO2,Temp,Timestamp`
- Baud rate : 9600

#### Phase 2 (Semaines 5-6) : Communication Sans Fil

**F2.1 - Transmission Bluetooth**
- Module HC-05 ou HC-06
- Appairage avec smartphone/PC
- Portée : 10-100 m
- Fréquence transmission : 1 fois par seconde

**F2.2 - Application Mobile Basique**
- Affichage graphique des 3 paramètres
- Historique des 1 dernière heure
- Notifications d'alertes
- Déconnexion/reconnexion automatique

#### Phase 3 (Semaines 7-8) : Traitement du Signal Avancé

**F3.1 - Filtrage Numérique**
- Filtre passe-bas (Butterworth ordre 2, fc=1Hz) pour bruit capteur
- Détection de pics pour fiabiliser le comptage BPM
- Analyse de fréquence (FFT) sur fenêtre 10 secondes

**F3.2 - Analyse Temporelle**
- Calcul variabilité fréquence cardiaque (HRV) sur 5 minutes
- Détection de tendances (augmentation/diminution)
- Historique sur 24 heures

**F3.3 - Stockage de Données**
- Sauvegarde sur carte microSD (24h de données)
- Format CSV pour analyse ultérieure

#### Phase 4 (Semaines 9-10) : Intelligence Artificielle

**F4.1 - Classification d'Anomalies**
- Détection d'arythmies cardiaques (ML classification)
- Algorithme SVM ou KNN entraîné offline
- Inférence sur Arduino Nano 33 BLE (TinyML)

**F4.2 - Alertes Intelligentes**
- Scores de risque basés sur combinaison de paramètres
- Prédiction de tendances
- Apprentissage des patterns individuels

### 2.3 Cas d'Utilisation Principaux

```
┌──────────────────────────────────────────────────────┐
│              CAS D'UTILISATION PRINC IPAUX             │
├──────────────────────────────────────────────────────┤
│
│ CU1: Patient prend ses mesures
│   • Allume le système
│   • Pose doigt sur capteur MAX30102 (5-10 sec)
│   • Voir résultats sur LCD
│   • Résultats transmis à l'appli mobile
│
│ CU2: Médecin consulte les données
│   • Se connecte à l'appli web/mobile
│   • Voit les dernières mesures du patient
│   • Accède à l'historique sur 7 jours
│   • Télécharge données pour analyse
│
│ CU3: Alerte anomalie détectée
│   • Système détecte SpO2 < 90%
│   • LED rouge s'allume + buzzer
│   • SMS/notification envoyés au médecin
│   • Historique sauvegardé
│
└──────────────────────────────────────────────────────┘
```

---

## 3. SPÉCIFICATIONS TECHNIQUES

### 3.1 Capteurs Biomédicaux

#### 3.1.1 MAX30102 - Oxymètre et Capteur de Fréquence Cardiaque

```
┌─────────────────────────────────────────────────────┐
│           MAX30102 PULSE OXIMETER SENSOR             │
├─────────────────────────────────────────────────────┤
│ Constructeur          : Maxim Integrated             │
│ Type                  : Capteur photopléthysmographe│
│ Paramètres mesurés    : BPM, SpO2                   │
│ Plage BPM             : 20-200 bpm                  │
│ Plage SpO2            : 0-100 %                     │
│ Précision SpO2        : ±2% (70-100%)               │
│ Précision BPM         : ±1 bpm                      │
│ Résolution            : 16 bits                     │
│ Fréquence ECH.        : 100-400 Hz (configurable)   │
│ Interface             : I2C (0x57)                  │
│ Tension alim.         : 1.8V - 3.3V                 │
│ Consommation          : 11mA (mode normal)          │
│                         0.7mA (mode sleep)          │
│ Dimensions            : 21×14×2.3 mm                │
│ Temps stabilisation   : 2-3 secondes                │
└─────────────────────────────────────────────────────┘
```

**Caractéristiques LED :**
- LED 1 (rouge 660nm) : pénètration superficielle, artères
- LED 2 (infrarouge 880nm) : pénètration plus profonde
- Mesure du ratio d'absorption pour calculer SpO2

**Câblage I2C :**
```
MAX30102    Arduino Uno
───────     ──────────
VCC     ──→ 3.3V (important : PAS 5V)
GND     ──→ GND
SDA     ──→ A4 (+ résistance 4.7kΩ si besoin)
SCL     ──→ A5 (+ résistance 4.7kΩ si besoin)
INT     ──→ (optionnel, pas utilisé Phase 1)
```

#### 3.1.2 MLX90614 - Capteur Infrarouge de Température

```
┌─────────────────────────────────────────────────────┐
│      MLX90614 IR TEMPERATURE SENSOR (SANS CONTACT)  │
├─────────────────────────────────────────────────────┤
│ Constructeur          : Melexis                     │
│ Type                  : Thermomètre infrarouge      │
│ Plage mesure ambiante : -40 à +125°C                │
│ Plage mesure objet    : -70 à +380°C                │
│ Résolution            : 0.01°C (numérique)          │
│ Précision             : ±0.5°C (0-50°C)             │
│ Angle de vision       : 5° (D:S = 1:1)              │
│ Distance mesure       : 1-10 cm optimal             │
│ Interface             : I2C / PWM                   │
│ Adresse I2C           : 0x5A (par défaut)           │
│ Tension alim.         : 2.6V - 3.6V (version Bxx)   │
│ Consommation          : 1.5 mA                      │
│ Temps réponse         : 100-200 ms                  │
│ Dimensions            : 3.0×3.0×3.5 mm              │
│ Package               : TO-39 (capteur sur module)   │
└─────────────────────────────────────────────────────┘
```

**Principe de mesure :**
- Détection rayonnement IR émis par la peau
- Compensation température ambiante automatique
- Calcul température à partir de spectral du rayonnement

**Câblage I2C :**
```
MLX90614    Arduino Uno
───────     ──────────
VCC     ──→ 3.3V
GND     ──→ GND
SDA     ──→ A4 (même ligne que MAX30102)
SCL     ──→ A5 (même ligne que MAX30102)
```

**Positionnement :** À 5-10 cm de la peau (front ou doigt)

### 3.2 Microcontrôleur Principal

#### Arduino Uno R3

```
┌─────────────────────────────────────────────────────┐
│         ARDUINO UNO R3 - PHASE 1 et 2                │
├─────────────────────────────────────────────────────┤
│ Processeur            : ATmega328P                  │
│ Fréquence             : 16 MHz                      │
│ Mémoire Flash         : 32 KB (programme)           │
│ RAM                   : 2 KB                        │
│ EEPROM                : 1 KB                        │
│ Entrées analogiques   : 6 (ADC 10 bits)             │
│ Sorties numériques    : 14                          │
│ Bus I2C               : SDA=A4, SCL=A5              │
│ UART Série            : TX=1, RX=0                  │
│ Alimentation          : 5V (USB ou barrel)          │
│ Consommation          : ~50 mA @ 5V                 │
│ Temps programme       : ~30 secondes (upload)       │
│ IDE                   : Arduino IDE 2.0+            │
└─────────────────────────────────────────────────────┘
```

**Limites Arduino Uno pour Phase 4 (ML) :**
- Mémoire RAM trop limitée pour TinyML avancé
- Solution : Migrer vers Arduino Nano 33 BLE pour Phase 4

#### Arduino Nano 33 BLE (Version Avancée - Phase 4)

```
Processeur        : ARM Cortex-M4 (64 MHz)
RAM               : 256 KB (vs 2KB sur Uno)
Flash             : 1 MB
Bluetooth         : Intégré
TinyML support    : Excellent
```

### 3.3 Affichage

#### LCD I2C 16×2

```
┌─────────────────────────────────────────────────────┐
│         LCD I2C 16×2 - AFFICHAGE LOCAL               │
├─────────────────────────────────────────────────────┤
│ Type              : Liquid Crystal Display (LCD)    │
│ Résolution        : 16 colonnes × 2 lignes          │
│ Interface         : I2C (PCF8574 backpack)          │
│ Adresse I2C       : 0x27 (standard)                 │
│ Couleur caract.   : Noir sur fond blanc/bleu        │
│ Éclairage         : LED bleue ajustable             │
│ Tension alim.     : 5V (via backpack I2C)           │
│ Consommation      : ~20 mA                          │
│ Format affichage  : "BPM:72 | SPO2:98%"             │
│                     "Temp:37.2C NORMAL"             │
└─────────────────────────────────────────────────────┘
```

**Câblage :**
```
LCD I2C      Arduino Uno
───────      ──────────
VCC      ──→ 5V
GND      ──→ GND
SDA      ──→ A4
SCL      ──→ A5
```

### 3.4 Modules de Communication

#### HC-05 Bluetooth Module (Phase 2)

```
┌─────────────────────────────────────────────────────┐
│         HC-05 BLUETOOTH SERIAL MODULE                │
├─────────────────────────────────────────────────────┤
│ Constructeur      : Zhuhai Jinou Electronic         │
│ Version Bluetooth : 2.0 (SPP profile)               │
│ Portée            : 10m (Low Power Mode)            │
│                    100m (High Power Mode)           │
│ Fréquence         : 2.4 GHz ISM                     │
│ Vitesse UART      : 9600 bps (configurable)         │
│ Adresse I2C       : N/A (interface UART)            │
│ Tension alim.     : 3.3V - 5V                       │
│ Consommation      : 8mA (idle)                      │
│                    20mA (TX)                        │
│ Brochages         : RX, TX, GND, VCC                │
│ Mode              : Esclave (reçoit connexions)     │
│ Nom Bluetooth     : "HC-05" (configurable)          │
│ Code PIN          : 1234 (configurable)             │
└─────────────────────────────────────────────────────┘
```

**Câblage :**
```
HC-05       Arduino Uno
────        ──────────
RX      ──→ TX (pin 1) via résistance diviseur
TX      ──→ RX (pin 0) via résistance diviseur
GND     ──→ GND
VCC     ──→ 5V
```

**Diviseur de tension (RX vers Arduino) :**
- Limite tension à 3.3V acceptable pour HC-05
- Diviseur : R1=2k2, R2=3k3 entre 5V et ground

### 3.5 Alimentation

#### Batterie LiPo 3S (Phase mobile)

```
Tension             : 11.1V nominal (10.5-12.6V)
Capacité            : 2000 mAh
Autonomie estimée   : 8-10 heures
Connecteur          : Deans/XT60 (standard)
```

#### USB Alimentation (Développement)

- Arduino Uno : 5V via câble USB Type-B
- LCD + capteurs : Dérivés de la sortie 5V Arduino
- HC-05 : Dérivé via diviseur tension

---

## 4. ARCHITECTURE DU SYSTÈME

### 4.1 Schéma Synoptique Global

```
╔════════════════════════════════════════════════════════════════╗
║                    SYSTÈME MULTIPARAMÉTRIQUE                  ║
╚════════════════════════════════════════════════════════════════╝

                      ┌─────────────────┐
                      │   CAPTEURS      │
                      ├─────────────────┤
                      │ • MAX30102      │  Biomédicaux
              ┌──────→│   (BPM, SpO2)   │
              │       │ • MLX90614      │
              │       │   (Température) │
              │       └─────────────────┘
              │
              │        ┌─────────────────┐
              │        │  ARDUINO UNO    │  Traitement
              │        │                 │  Local
              ├───────→│  • I2C master   │
              │        │  • Processing   │
              │        │  • Alertes      │
              │        └─────────────────┘
              │        ┌─────────────────┐
              │        │  AFFICHAGE      │  Retour
              │        │                 │  Utilisateur
              ├───────→│ • LCD I2C 16×2  │
              │        │ • LEDs (alerte) │
              │        │ • Buzzer        │
              │        └─────────────────┘
              │
              │        ┌─────────────────┐
              │        │  COMMUNICATION  │  Transmission
              │        │                 │  Données
              └───────→│ • HC-05 BT      │
                       │ • Série USB     │
                       │ (Phase 2)       │
                       └─────────────────┘
                              ↓
                       ┌──────────────────┐
                       │  SMARTPHONE/PC   │
                       │                  │
                       │ • App mobile     │
                       │ • Dashboard web  │
                       └──────────────────┘
```

### 4.2 Topologie I2C Bus

```
Arduino Uno (Master)
       │
       ├─── SDA (A4) ──────┬──────┬──────┐
       │                   │      │      │
       ├─── SCL (A5) ──────┼──────┼──────┤
       │                   │      │      │
       GND ────────────────┴──────┴──────┤
                          │      │      │
                    MAX30102  MLX90614 LCD-I2C
                    (0x57)    (0x5A)   (0x27)

Pullups : 4.7kΩ chaque ligne (généralement intégrés)
```

### 4.3 Flux de Données

```
Cycle d'acquisition et traitement (1 seconde) :

T0: Lecture capteurs
    ├─ MAX30102 (I2C) → BPM, SpO2
    └─ MLX90614 (I2C) → Temp

T1: Traitement local
    ├─ Moyenne mobile (5 dern. mesures)
    ├─ Détection anomalies
    └─ Stockage buffer

T2: Affichage
    ├─ LCD I2C
    ├─ LEDs état (vert=ok, orange=warning, rouge=alerte)
    └─ Buzzer si alerte

T3: Communication
    ├─ UART série → ordinateur (débogage)
    └─ HC-05 BT → smartphone/cloud

T4: Stockage (optionnel SD)
    └─ Données + timestamp

[Fin du cycle → Retour à T0+1s]
```

### 4.4 Hiérarchie Logicielle

```
main (loop Arduino)
│
├─ initCapteurs()              [Setup]
│  ├─ setupI2C()
│  ├─ setupMAX30102()
│  └─ setupMLX90614()
│
├─ initAffichage()             [Setup]
│  ├─ setupLCD()
│  └─ setupLEDs()
│
├─ lectureMAX30102()           [Loop - 1Hz]
│  ├─ readRegister(...)
│  ├─ calculateBPM()
│  └─ calculateSpO2()
│
├─ lectureMLX90614()           [Loop - 1Hz]
│  ├─ readObjectTemp()
│  └─ readAmbientTemp()
│
├─ traitementSignal()          [Loop - 1Hz]
│  ├─ moyenneMobile(5)
│  ├─ detectionAnomalies()
│  └─ updateLEDsAlerte()
│
├─ affichageLCD()              [Loop - 1Hz]
│  ├─ formatString()
│  └─ printLCD()
│
├─ transmissionDonnees()       [Loop - 1Hz]
│  ├─ printSerial()
│  └─ sendBluetoothHC05()
│
└─ gestionAlertes()            [Loop - immediate]
   ├─ activateBuzzer()
   └─ logEvent()
```

---

## 5. CALENDRIER ET JALONS

### 5.1 Planning Détaillé (10 semaines)

```
┌─────────────────────────────────────────────────────────────┐
│                    GANTT - PLANNING PROJET                  │
└─────────────────────────────────────────────────────────────┘

PHASE 1 : CAPTEUR MAX30102 (Semaines 1-2)
├─ S1.1 : Étude datasheet + théorie I2C
│         └─ Livrable : Document technique I2C
├─ S1.2 : Câblage prototypage + test matériel
│         └─ Livrable : Schéma électrique validé
├─ S1.3 : Code Arduino MAX30102 basique
│         └─ Livrable : Sketch fonctionnel v0.1
└─ S1.4 : Validation mesures + étalonnage
          └─ Livrable : Courbes de validation

PHASE 2 : CAPTEUR MLX90614 (Semaines 3-4)
├─ S2.1 : Étude datasheet MLX90614
│         └─ Livrable : Notes techniques
├─ S2.2 : Intégration I2C multi-capteurs
│         └─ Livrable : Code v0.2 (2 capteurs)
├─ S2.3 : LCD I2C affichage
│         └─ Livrable : Code v0.3 (+ LCD)
└─ S2.4 : Tests système complet Phase 1
          └─ Livrable : Rapport test + photographies

PHASE 3 : COMMUNICATION SANS FIL (Semaines 5-6)
├─ S3.1 : Intégration HC-05 + série
│         └─ Livrable : Code v0.4 (+ Bluetooth)
├─ S3.2 : App mobile basique (Android/iOS)
│         └─ Livrable : App prototype
├─ S3.3 : Tests transmission
│         └─ Livrable : Logs transmission
└─ S3.4 : Documentation utilisateur
          └─ Livrable : Guide d'usage

PHASE 4 : TRAITEMENT SIGNAL (Semaines 7-8)
├─ S4.1 : Implémentation filtres (Butterworth)
│         └─ Livrable : Bibliothèque DSP
├─ S4.2 : Détection pics / FFT basique
│         └─ Livrable : Code v0.5 (+ signal processing)
├─ S4.3 : Stockage SD + historique
│         └─ Livrable : Données sauvegardées
└─ S4.4 : Analyse fréquentielle des données
          └─ Livrable : Graphiques spectres

PHASE 5 : ML & IA (Semaines 9-10)
├─ S5.1 : Collecte dataset + labellisation
│         └─ Livrable : Dataset 500+ mesures
├─ S5.2 : Entraînement modèles ML (offline)
│         └─ Livrable : Modèle SVM/KNN + scores
├─ S5.3 : Déploiement TinyML Arduino Nano 33 BLE
│         └─ Livrable : Code v1.0 avec ML
├─ S5.4 : Tests validation
│         └─ Livrable : Rapport précision/rappel
└─ S5.5 : Documentation finale + soutenance
          └─ Livrable : Rapport complet 40+ pages
```

### 5.2 Jalons Clés (Milestones)

| Semaine | Jalon | Critères | Responsable |
|---------|-------|----------|-------------|
| S1 fin | MVP Phase 1 ✓ | MAX30102 lisant BPM+SpO2 | Électronique |
| S2 fin | MVP Phase 2 ✓ | LCD affichant 3 paramètres | Électronique |
| S3 fin | MVP Phase 3 ✓ | Transmission BT fonctionnelle | Software |
| S4 fin | MVP Phase 4 ✓ | Filtrage et historique OK | DSP |
| S5 fin | MVP final + rapport | Démo complète + doc | Tous |

---

## 6. PLAN D'ACQUISITION

### 6.1 Composants Électroniques

#### Quantités et Coûts Estimés

| Référence | Composant | Quantité | Prix Unitaire | Prix Total | Fournisseur |
|-----------|-----------|----------|---------------|------------|-------------|
| ARD-UNO | Arduino Uno R3 | 1 | 25€ | 25€ | AliExpress/Boutique |
| MAX30102 | Module MAX30102 | 1 | 12€ | 12€ | AliExpress |
| MLX90614 | Module MLX90614 | 1 | 15€ | 15€ | AliExpress |
| LCD-I2C | LCD 16×2 I2C | 1 | 8€ | 8€ | AliExpress |
| HC-05 | Module HC-05 | 1 | 8€ | 8€ | AliExpress |
| BREADBOARD | Breadboard 830pts | 1 | 5€ | 5€ | Local |
| JUMPER | Câbles Dupont (50x) | 1 | 4€ | 4€ | Local |
| LED | LEDs 5mm + résistances | 5 | 0.50€ | 2.50€ | Local |
| BUZZER | Buzzer 5V | 1 | 2€ | 2€ | Local |
| RESISTOR | Résistances assortis | - | - | 3€ | Local |
| BATTERY | Batterie 9V (proto) | 2 | 3€ | 6€ | Local |
| USB | Câbles USB Type-B | 2 | 3€ | 6€ | Local |
| **TOTAL PHASE 1-2** | | | | **96.50€** | |
| **PHASE 4 (optionnel)** | | | | | |
| NANO33BLE | Arduino Nano 33 BLE | 1 | 30€ | 30€ | AliExpress |
| SD-MODULE | Module Lecteur SD | 1 | 5€ | 5€ | AliExpress |
| SDCARD | Carte SD 16GB | 1 | 5€ | 5€ | Local |
| **TOTAL PHASE 4** | | | | **40€** | |
| **GRAND TOTAL** | | | | **136.50€** | |

### 6.2 Logiciels & Outils (Gratuits)

- Arduino IDE 2.0+ (opensource)
- Python 3.x + matplotlib (analyse données)
- MIT App Inventor (app mobile)
- Processing (visualisation)
- KiCad (schémas PCB)
- Git + GitHub (contrôle version)

### 6.3 Documentation à Acquérir

| Document | Source | Format |
|----------|--------|--------|
| Datasheet MAX30102 | Maxim Integrated | PDF |
| Datasheet MLX90614 | Melexis | PDF |
| Arduino Reference | Arduino.cc | Web |
| I2C Protocol Spec | NXP/Wikipedia | PDF |
| Signal Processing | DSPGuru.com | Web |

---

## 7. CRITÈRES D'ACCEPTATION

### 7.1 Critères Fonctionnels

#### Phase 1 (MAX30102)

- [x] MAX30102 détecté sur bus I2C (commande I2C scanner)
- [x] Lecture BPM : valeur entre 40-180 bpm
- [x] Lecture SpO2 : valeur entre 70-100 %
- [x] Stabilité : variation < 2% entre 2 mesures (même patient, 1s intervalle)
- [x] Temps réponse : < 3 secondes pour résultat stable
- [x] Affichage LCD : 3 paramètres visibles et lisibles

#### Phase 2 (MLX90614)

- [x] MLX90614 détecté sur bus I2C à l'adresse 0x5A
- [x] Température mesurée : plage 35-40°C (corps humain)
- [x] Précision : ±0.5°C validée contre thermomètre de référence
- [x] Compensation température ambiante OK
- [x] LCD affiche 3 paramètres simultanément

#### Phase 3 (Bluetooth)

- [x] HC-05 appairé au smartphone/PC
- [x] Transmission de 1 donnée/seconde
- [x] Portée minimale : 5 m sans obstacle
- [x] Perte de paquets : < 1% sur 1 minute
- [x] Reconnexion automatique après déconnexion

#### Phase 4 (Traitement Signal)

- [x] Moyenne mobile lisse les variations (passband ripple < 3dB)
- [x] Détection pics robuste (> 95% de détection vrais positifs)
- [x] Alertes déclenchées au bon seuil (±2%)
- [x] Historique 24h stocké sans corruption

#### Phase 5 (ML)

- [x] Modèle ML entraîné (accuracy > 90%)
- [x] Inférence TinyML < 100ms
- [x] Détection anomalies fonctionnelle

### 7.2 Critères Non-Fonctionnels

| Critère | Objectif | Mesure |
|---------|----------|--------|
| **Précision** | ±2% pour SpO2, ±1 BPM, ±0.5°C | Test contre appareils certifiés |
| **Fiabilité** | 99% uptime en usage normal | Test 24h continu |
| **Latence** | < 1 sec affichage après mesure | Chronomètre |
| **Consommation** | < 500 mA @ 5V | Ampèremètre |
| **Portée Bluetooth** | ≥ 5m sans ligne directe | Test distance |
| **Convivialité** | Utilisable sans formation | Test utilisateur externe |
| **Coût** | < 150€ pour prototype | Factures composants |

### 7.3 Tests de Validation

#### Tests Unitaires (par composant)

```
MAX30102:
  - I2C Communication Test
  - LED detect Test
  - BPM Range Test [-40, 40-180, >180]
  - SpO2 Range Test [<70, 70-100, >100]

MLX90614:
  - I2C Communication Test
  - Temperature Range Test [<0°C, 35-40°C, >45°C]
  - Precision Test (±0.5°C vs référence)

LCD I2C:
  - Display Test (tous caractères)
  - Refresh Rate Test

HC-05:
  - Pairing Test
  - Connection Test
  - Data Transmission Test
  - Range Test [<1m, 5m, 10m, >100m]
```

#### Tests d'Intégration

- ✓ Tous capteurs simultanément
- ✓ Lecture + affichage + transmission
- ✓ Cycles 1 heure continue
- ✓ Alertes déclenchées correctement

#### Tests de Validation Clinique (Bonus)

- ✓ Comparaison avec oxymètre de référence (10 mesures)
- ✓ Comparaison avec thermomètre IR médical (10 mesures)
- ✓ Test variabilité intra-patient (même personne, 3 sessions)
- ✓ Test variabilité inter-patient (5 personnes)

---

## 8. RISQUES ET MITIGATIONS

### 8.1 Risques Techniques

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|-----------|
| **Bus I2C non stable** | Moyenne | Haut | Plan câblage précoce + pullups |
| **MAX30102 incompatibilité voltage** | Faible | Haut | Régulateur 3.3V (LDO) |
| **Signal bruité (MAX30102)** | Moyenne | Moyen | Filtrage numérique + blindage câbles |
| **Portée Bluetooth insuffisante** | Faible | Moyen | Antenne externe HC-05 |
| **Dérive température MLX90614** | Faible | Faible | Étalonnage mensuel |
| **Débordement mémoire Arduino Uno** | Moyen | Haut | Tests mémoire réguliers |

### 8.2 Risques Planification

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|-----------|
| **Délai livraison composants** | Moyenne | Haut | Commander 2 semaines avant |
| **Composant défectueux** | Faible | Moyen | Avoir 2 exemplaires MAX30102 |
| **Manque de compétence I2C** | Faible | Haut | Formations + ressources web |
| **Surcharge tâches Phase 5** | Moyenne | Moyen | Commencer ML tôt (S4) |

### 8.3 Risques de Sécurité

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|-----------|
| **Données sensibles patient** | Moyenne | Haut | Chiffrement BT (futur) |
| **Court-circuit alimentation** | Faible | Moyen | Protection fusible (futur) |
| **Surchauffe composants** | Très faible | Faible | Test thermographie |

---

## 9. RESSOURCES ET BUDGET

### 9.1 Ressources Humaines

| Rôle | Personne | Charge | Semaines |
|------|----------|--------|----------|
| **Chef Projet** | 1 personne | 30% | 10 |
| **Électronique** | 1 personne | 50% | 10 |
| **Software/Firmware** | 1 personne | 50% | 10 |
| **DSP/Algo** | 1 personne | 30% | 8 |
| **Test/Validation** | 1 personne | 20% | 10 |

**Total ETP :** 1.8 ETP sur 10 semaines = ~18 semaines-homme

### 9.2 Budget Détaillé

#### Budget Composants (Matériel)

```
Électronique            : 96.50€
Phase 4 optionnel       : 40.00€
Réserve (10%)           : 13.65€
────────────────────────────────
TOTAL MATÉRIEL          : 150.15€
```

#### Budget Logiciels & Services

```
Outils (gratuits)       : 0€
Hébergement données     : 0€ (prototype)
Formations              : 0€ (web + auto-formation)
────────────────────────────────
TOTAL LOGICIEL          : 0€
```

#### Budget Ressources (Temps)

```
18 semaines-homme × tarif étudiant/stagiaire
(Hypothèse : sans coûts directs en formation BTS)
```

### 9.3 Ressources Matérielles Disponibles

- ✅ Laboratoires électronique (soudeuse, multimètre)
- ✅ Ordinateurs pour développement (Arduino IDE)
- ✅ Accès internet (documentation en ligne)
- ✅ Salle de test (pour mesures)

### 9.4 Ressources Informationnelles

| Type | Source |
|------|--------|
| Tutoriels I2C | Adafruit, Arduino forums |
| Datasheets | Constructeurs (PDF) |
| Bibliothèques Arduino | GitHub + Arduino Library Manager |
| Traitement Signal | DSPGuru, coursera |
| ML Arduino | Edge Impulse, TensorFlow Lite |

---

## 10. LIVRABLES FINAUX

### 10.1 Documents

1. **Cahier des Charges** (ce document) - 30+ pages
2. **Rapport Technique** - 40+ pages
   - Architecture système détaillée
   - Schémas électriques (Fritzing/KiCad)
   - Algorithmes traitement signal
   - Résultats tests et validation
3. **Manuel Utilisateur** - 10 pages
4. **Guide Installation** - 5 pages
5. **Code Source Commenté** - GitHub repository
6. **Dataset de Validation** - CSV 500+ mesures
7. **Slides Présentation** - 20 slides

### 10.2 Prototype Matériel

- ✅ Boîtier prototype (carton/impression 3D)
- ✅ Schéma câblage final
- ✅ PCB prototype (vérouillage)

### 10.3 Code & Applications

- ✅ Firmware Arduino complet (v1.0)
- ✅ Application mobile (APK)
- ✅ Dashboard web (HTML/JavaScript)
- ✅ Scripts analyse données (Python)
- ✅ Modèles ML (TensorFlow Lite)

### 10.4 Vidéos & Démonstrations

- ✅ Démo fonctionnelle 5 min
- ✅ Tutoriel mise en place 10 min
- ✅ Walkthrough code 15 min

---

## ANNEXES

### A. Glossaire Technique

| Terme | Définition |
|-------|-----------|
| **BPM** | Beats Per Minute - Fréquence cardiaque |
| **SpO2** | Saturation en oxygène artériel (%) |
| **PPG** | Photopléthysmographie (technique LED) |
| **I2C** | Inter-Integrated Circuit (bus communication) |
| **PWM** | Pulse Width Modulation |
| **DSP** | Digital Signal Processing |
| **HRV** | Heart Rate Variability |
| **FFT** | Fast Fourier Transform |
| **TinyML** | Machine Learning sur microcontrôleurs |
| **Edge Computing** | Traitement données localement |

### B. Références Normatives

- IEC 60601-1 : Sécurité dispositifs médicaux
- ISO 14971 : Gestion risques
- RGPD : Protection données personnelles
- IEEE 802.15.1 : Spécifications Bluetooth

### C. Ressources Web

- Arduino Official : https://www.arduino.cc
- Adafruit Tutorials : https://learn.adafruit.com
- GitHub MAX30102 : https://github.com/sparkfun/SparkFun_MAX3010x
- MLX90614 Guide : https://wiki.dfrobot.com/MLX90614_IR_Temperature_Sensor_SKU_SEN0142
- TensorFlow Lite Micro : https://www.tensorflow.org/lite/microcontrollers
- Signal Processing : https://www.dspguru.com

---

**Document révisé : 18 Décembre 2025**  
**Version : 1.0 - Final**  
**Statut : Approuvé pour démarrage Phase 1**
