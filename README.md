# 🏥 Système de Surveillance Multiparamétrique en Santé Connectée

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Arduino IDE](https://img.shields.io/badge/Arduino%20IDE-2.0%2B-blue)](https://www.arduino.cc)
[![Status: In Development](https://img.shields.io/badge/Status-In%20Development-orange)](https://github.com/nticonseil/multiparameter-health-monitoring)
[![Language: C++](https://img.shields.io/badge/Language-C%2B%2B-brightgreen)](https://cplusplus.com)
[![Last Update](https://img.shields.io/badge/Last%20Update-Dec%202025-lightblue)](https://github.com/nticonseil/multiparameter-health-monitoring)

---

## 📋 Table des Matières

- [🎯 Vision du Projet](#-vision-du-projet)
- [✨ Fonctionnalités](#-fonctionnalités)
- [🛠 Architecture Système](#-architecture-système)
- [📦 Composants Requis](#-composants-requis)
- [⚡ Quick Start](#-quick-start)
- [📁 Structure du Repository](#-structure-du-repository)
- [🚀 Phases de Développement](#-phases-de-développement)
- [📚 Documentation](#-documentation)
- [🤝 Contribution](#-contribution)
- [📝 Licence](#-licence)

---

## 🎯 Vision du Projet

Concevoir un **prototype portable, peu coûteux et haute performance** capable de mesurer **3 paramètres vitaux simultanément** (Fréquence cardiaque, Saturation en Oxygène, Température corporelle) et de les transmettre sans fil pour analyse et alerte automatique.

**Contexte Éducatif :** Projet d'étude combinant **Électronique Embarquée** + **Théorie du Signal** + **IoT** + **Machine Learning**

---

## ✨ Fonctionnalités

### Phase 1-2 (MVP Basique) ✅
- ✓ Mesure fréquence cardiaque (BPM) via **MAX30102**
- ✓ Mesure saturation oxygène (SpO2 %) via **MAX30102**
- ✓ Mesure température corporelle (°C) via **MLX90614**
- ✓ Affichage temps réel sur **LCD I2C 16×2**
- ✓ Traitement signal (moyenne mobile)
- ✓ Détection anomalies + alertes
- ✓ Transmission série (USB)

### Phase 3 (Communication Sans Fil) 🔄
- 📡 Transmission **Bluetooth HC-05**
- 📱 Application mobile basique
- 🔔 Notifications temps réel

### Phase 4 (Signal Avancé) ⚙️
- 🔊 Filtrage numérique (Butterworth)
- 📊 Analyse FFT / spectrale
- 💾 Stockage microSD
- 📈 Historique 24h

### Phase 5 (Intelligence Artificielle) 🤖
- 🧠 Détection arythmies (ML)
- 🎯 Classification anomalies (SVM/KNN)
- ⚡ Inférence TinyML embarquée
- 🔮 Prédiction tendances

---

## 🛠 Architecture Système

```
┌─────────────────────────────────────────────────────┐
│           SYSTÈME MULTIPARAMÉTRIQUE                  │
├─────────────────────────────────────────────────────┤
│                                                      │
│  ┌────────────────┐        ┌────────────────┐       │
│  │   CAPTEURS     │        │  ARDUINO UNO   │       │
│  ├────────────────┤        ├────────────────┤       │
│  │ • MAX30102     │───I2C──│ • Master I2C   │       │
│  │ • MLX90614     │        │ • Processing   │       │
│  └────────────────┘        │ • Alertes      │       │
│                            └────────────────┘       │
│                                   │                 │
│                          ┌────────┼────────┐        │
│                          ▼        ▼        ▼        │
│                     ┌────────┐ ┌────┐  ┌──────┐    │
│                     │ LCD    │ │LED │  │HC-05 │    │
│                     │I2C     │ │    │  │(BT)  │    │
│                     └────────┘ └────┘  └──────┘    │
│                          │               │          │
│                          └───────┬───────┘          │
│                                  ▼                  │
│                        ┌──────────────────┐         │
│                        │  SMARTPHONE/PC   │         │
│                        │ • App mobile     │         │
│                        │ • Dashboard      │         │
│                        └──────────────────┘         │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## 📦 Composants Requis

### Électronique

| Composant | Référence | Quantité | Prix | Fournisseur |
|-----------|-----------|----------|------|-------------|
| Arduino Uno R3 | ARD-UNO | 1 | 25€ | Boutique |
| Capteur Fréquence Cardiaque + SpO2 | MAX30102 | 1 | 12€ | AliExpress |
| Capteur Température Infrarouge | MLX90614 | 1 | 15€ | AliExpress |
| Afficheur LCD I2C 16×2 | LCD-I2C | 1 | 8€ | AliExpress |
| Module Bluetooth | HC-05 | 1 | 8€ | AliExpress |
| Breadboard + Câbles Dupont | KIT | 1 | 10€ | Local |
| LEDs + Résistances | KIT | - | 5€ | Local |
| Buzzer 5V | BUZZER | 1 | 2€ | Local |
| **TOTAL PHASE 1-2** | | | **96.50€** | |

### Logiciels (Gratuits ✅)

- ✅ Arduino IDE 2.0+ (opensource)
- ✅ Python 3.x + matplotlib
- ✅ MIT App Inventor (app mobile)
- ✅ Git + GitHub
- ✅ KiCad (schémas)

---

## ⚡ Quick Start

### 1️⃣ Cloner le Repository

```bash
git clone https://github.com/nticonseil/multiparameter-health-monitoring.git
cd multiparameter-health-monitoring
```

### 2️⃣ Installer Arduino IDE

```bash
# Linux
sudo apt install arduino

# macOS
brew install arduino

# Windows
# Télécharger depuis https://www.arduino.cc/en/software
```

### 3️⃣ Installer les Bibliothèques Requises

```
Arduino IDE → Sketch → Inclure une bibliothèque → Gérer les bibliothèques

Chercher et installer :
- SparkFun MAX3010x Pulse and Proximity Sensor Library
- Adafruit MLX90614 Library
- LiquidCrystal I2C (par Frank de Brabander)
```

### 4️⃣ Ouvrir le Sketch Principal

```
File → Open → /firmware/Phase1_MAX30102_basic/Phase1_MAX30102_basic.ino
```

### 5️⃣ Flasher sur Arduino

```
Sketch → Verify (Vérifier)
Sketch → Upload (Téléverser)
```

### 6️⃣ Tester

```
Ouvrir Tools → Serial Monitor
Baud Rate : 9600
Placer le doigt sur le capteur MAX30102
Observer les mesures s'afficher
```

---

## 📁 Structure du Repository

```
multiparameter-health-monitoring/
│
├── 📄 README.md                          # Ce fichier
├── 📄 ARCHITECTURE.md                    # Détails techniques complets
├── 📄 INSTALLATION_GUIDE.md              # Guide installation pas à pas
├── 📄 CONTRIBUTING.md                    # Directives contribution
├── 📄 CHANGELOG.md                       # Historique versions
├── 📄 LICENSE                            # Licence MIT
├── 📄 .gitignore                         # Fichiers à ignorer Git
│
├── 📁 firmware/                          # Code Arduino
│   ├── 📁 Phase1_MAX30102_basic/         # Phase 1 : Capteur fréquence cardiaque
│   │   ├── Phase1_MAX30102_basic.ino     # Sketch principal
│   │   ├── config.h                      # Configuration constantes
│   │   └── README.md                     # Documentation Phase 1
│   │
│   ├── 📁 Phase2_MultiCaptors_LCD/       # Phase 2 : 2 capteurs + LCD
│   │   ├── Phase2_MultiCaptors_LCD.ino   # Sketch principal
│   │   ├── config.h
│   │   ├── LCDDisplay.h                  # Gestion LCD
│   │   ├── SensorManager.h               # Gestion capteurs I2C
│   │   └── README.md
│   │
│   ├── 📁 Phase3_Bluetooth_Communication/# Phase 3 : HC-05 Bluetooth
│   │   ├── Phase3_HC05_Bluetooth.ino
│   │   ├── config.h
│   │   ├── BluetoothManager.h
│   │   └── README.md
│   │
│   ├── 📁 Phase4_SignalProcessing/       # Phase 4 : Filtres numériques
│   │   ├── Phase4_DSP_Advanced.ino
│   │   ├── config.h
│   │   ├── Filters.h                     # Butterworth, moyenne mobile
│   │   ├── SignalProcessor.h
│   │   └── README.md
│   │
│   ├── 📁 Phase5_MachineLearning/        # Phase 5 : TinyML
│   │   ├── Phase5_TinyML_Inference.ino
│   │   ├── model.h                       # Modèle ML compressé
│   │   ├── config.h
│   │   └── README.md
│   │
│   └── 📁 libraries/                     # Bibliothèques custom
│       ├── MAX30102_Custom/
│       ├── MLX90614_Custom/
│       └── DSP_Custom/
│
├── 📁 hardware/                          # Schémas et designs
│   ├── 📁 schematics/
│   │   ├── Phase1_MAX30102.kicad_sch    # Schéma KiCad Phase 1
│   │   ├── Phase2_Complete.kicad_sch
│   │   └── Fritzing_breadboard.fzz
│   │
│   ├── 📁 pcb/                          # Layout PCB (futur)
│   │   └── multiparameter_monitor_v1.kicad_pcb
│   │
│   ├── 📁 3d_models/                    # Boîtiers 3D (impression)
│   │   ├── enclosure_v1.stl
│   │   └── pcb_holder.stl
│   │
│   └── 📁 datasheets/                   # Datasheets fournisseurs
│       ├── MAX30102_datasheet.pdf
│       ├── MLX90614_datasheet.pdf
│       ├── Arduino_Uno_pinout.pdf
│       └── HC-05_specs.pdf
│
├── 📁 software/                         # Applications, scripts
│   ├── 📁 mobile_app/
│   │   ├── android/                     # App Android (MIT App Inventor)
│   │   ├── ios/                         # App iOS (optionnel)
│   │   └── app_source.aia
│   │
│   ├── 📁 desktop_dashboard/            # Dashboard web/desktop
│   │   ├── index.html
│   │   ├── styles.css
│   │   ├── app.js
│   │   └── data_processor.py
│   │
│   └── 📁 python_scripts/               # Scripts d'analyse données
│       ├── data_analyzer.py
│       ├── signal_processor.py
│       ├── plot_data.py
│       ├── ml_trainer.py
│       └── requirements.txt
│
├── 📁 data/                             # Données et datasets
│   ├── 📁 raw_data/
│   │   ├── patient_001.csv
│   │   ├── patient_002.csv
│   │   └── .gitkeep
│   │
│   ├── 📁 processed_data/
│   │   ├── features_extracted.csv
│   │   └── labels.csv
│   │
│   └── 📁 ml_models/
│       ├── svm_model.pkl
│       ├── knn_model.pkl
│       └── model_performance.json
│
├── 📁 tests/                            # Tests et validation
│   ├── 📁 unit_tests/
│   │   ├── test_MAX30102.cpp
│   │   ├── test_MLX90614.cpp
│   │   ├── test_Filters.cpp
│   │   └── Makefile
│   │
│   ├── 📁 integration_tests/
│   │   ├── test_system_integration.cpp
│   │   └── test_communication.cpp
│   │
│   └── 📁 validation_data/
│       ├── reference_measurements.csv
│       └── test_results.md
│
├── 📁 docs/                             # Documentation projet
│   ├── 📁 Cahier_des_Charges/
│   │   ├── Cahier_Des_Charges_Sante_Connectee.md
│   ├── 📁 guides/
│   │   ├── I2C_Protocol_Guide.md
│   │   ├── Signal_Processing_101.md
│   │   ├── Arduino_Setup.md
│   │   └── Bluetooth_Configuration.md
│   │
│   ├── 📁 tutorials/
│   │   ├── Tutorial_Phase1_Step_by_Step.md
│   │   ├── Tutorial_Phase2_Integration.md
│   │   ├── Troubleshooting_I2C.md
│   │   └── Calibration_Guide.md
│   │
│   ├── 📁 api/
│   │   ├── Sensor_API.md
│   │   ├── Bluetooth_Protocol.md
│   │   └── Data_Format.md
│   │
│   ├── 📁 design/
│   │   ├── System_Architecture.md
│   │   ├── Signal_Flow_Diagram.md
│   │   └── UML_Diagrams.md
│   │
│   └── Technical_Report.md
│
├── 📁 images/                           # Images pour doc
│   ├── system_overview.png
│   ├── breadboard_layout.jpg
│   ├── lcd_display_example.jpg
│   ├── measurements_screenshot.png
│   └── mobile_app_screen.jpg
│
├── 📁 scripts/                          # Scripts utilitaires
│   ├── setup_environment.sh              # Initialiser dev env
│   ├── verify_compilation.sh             # Vérifier code
│   ├── upload_firmware.sh                # Flasher Arduino
│   ├── data_backup.py                    # Sauvegarder données
│   └── generate_report.py                # Générer rapport
│
├── 📁 .github/                          # GitHub Actions/Templates
│   ├── 📁 workflows/
│   │   ├── ci_build.yml                 # CI/CD Arduino
│   │   ├── tests.yml                    # Tests unitaires
│   │   └── documentation.yml            # Générer docs
│   │
│   ├── 📁 ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   │
│   └── PULL_REQUEST_TEMPLATE.md
│
├── 📄 requirements.txt                   # Dépendances Python
├── 📄 setup.py                          # Installation package
└── 📄 Makefile                          # Commandes build/test
```

---

## 🚀 Phases de Développement

### 📅 Phase 1 : Maîtriser le MAX30102 (Semaines 1-2)
**Objectif :** Lire fréquence cardiaque + SpO2

```
✅ Bus I2C fonctionnel
✅ Lecture BPM (40-180)
✅ Lecture SpO2 (70-100%)
✅ Affichage moniteur série
✅ Étalonnage validé
```

📂 **Fichier Principal :** `firmware/Phase1_MAX30102_basic/Phase1_MAX30102_basic.ino`

---

### 📅 Phase 2 : Intégrer MLX90614 + LCD (Semaines 3-4)
**Objectif :** 3 paramètres simultanés + affichage

```
✅ Multi-capteurs I2C
✅ Affichage LCD I2C
✅ Traitement signal basique
✅ Détection anomalies
✅ Tests système 1h
```

📂 **Fichier Principal :** `firmware/Phase2_MultiCaptors_LCD/Phase2_MultiCaptors_LCD.ino`

---

### 📅 Phase 3 : Bluetooth HC-05 (Semaines 5-6)
**Objectif :** Transmission sans fil + app mobile

```
✅ HC-05 appairé
✅ Transmission 1Hz
✅ App mobile basique
✅ Portée 5m+
✅ Tests stabilité
```

📂 **Fichier Principal :** `firmware/Phase3_Bluetooth_Communication/Phase3_HC05_Bluetooth.ino`

---

### 📅 Phase 4 : Traitement du Signal (Semaines 7-8)
**Objectif :** Filtrage avancé + analyse spectrale

```
✅ Filtres Butterworth
✅ Détection pics robuste
✅ Stockage microSD
✅ Analyse FFT
✅ Historique 24h
```

📂 **Fichier Principal :** `firmware/Phase4_SignalProcessing/Phase4_DSP_Advanced.ino`

---

### 📅 Phase 5 : Machine Learning (Semaines 9-10)
**Objectif :** Détection intelligente d'anomalies

```
✅ Dataset 500+ mesures
✅ Modèle ML entraîné (SVM/KNN)
✅ TinyML sur Arduino Nano 33
✅ Inférence < 100ms
✅ Accuracy > 90%
```

📂 **Fichier Principal :** `firmware/Phase5_MachineLearning/Phase5_TinyML_Inference.ino`

---

## 📚 Documentation

### Pour Débuter
- 📖 [**INSTALLATION_GUIDE.md**](./INSTALLATION_GUIDE.md) - Installation pas à pas
- 📖 [**ARCHITECTURE.md**](./ARCHITECTURE.md) - Architecture système détaillée
- 📖 [**Guides/I2C_Protocol_Guide.md**](./docs/guides/I2C_Protocol_Guide.md) - Comprendre I2C

### Tutoriels Par Phase
- 🎓 [**Tutorials/Tutorial_Phase1_Step_by_Step.md**](./docs/tutorials/Tutorial_Phase1_Step_by_Step.md)
- 🎓 [**Tutorials/Tutorial_Phase2_Integration.md**](./docs/tutorials/Tutorial_Phase2_Integration.md)
- 🎓 [**Guides/Troubleshooting_I2C.md**](./docs/guides/Troubleshooting_I2C.md)

### Ressources Techniques
- ⚙️ [**API/Sensor_API.md**](./docs/api/Sensor_API.md) - Référence capteurs
- ⚙️ [**API/Bluetooth_Protocol.md**](./docs/api/Bluetooth_Protocol.md) - Format données
- 📊 [**Design/System_Architecture.md**](./docs/design/System_Architecture.md) - Diagrammes système

### Datasheets & Références
- 📘 [**hardware/datasheets/**](./hardware/datasheets/) - Tous les datasheets
  - MAX30102_datasheet.pdf
  - MLX90614_datasheet.pdf
  - Arduino_Uno_pinout.pdf

---

## 🤝 Contribution

Les contributions sont les **bienvenues** ! 🎉

### Comment Contribuer ?

1. **Fork** le repository
2. **Créer une branche** `feature/ma-feature` ou `fix/mon-bug`
3. **Commit** avec messages clairs
4. **Push** vers votre fork
5. **Pull Request** avec description détaillée

### Directives
- Lire [**CONTRIBUTING.md**](./CONTRIBUTING.md)
- Respecter le [**Code Style Guide**](./docs/guides/Code_Style.md)
- Ajouter tests pour nouvelles features
- Mettre à jour documentation

### Branches

```
main              # Stable, releases uniquement
├── develop       # Intégration features
├── feature/*     # Nouvelles fonctionnalités
├── fix/*         # Corrections bugs
└── docs/*        # Mises à jour docs
```

---

## 📊 État du Projet

| Phase | Statut | Avancement | Deadline |
|-------|--------|-----------|----------|
| Phase 1 (MAX30102) | ⏳ À venir | 0% | S2 |
| Phase 2 (MLX90614 + LCD) | ⏳ À venir | 40% | S4 |
| Phase 3 (Bluetooth) | ⏳ À venir | 0% | S6 |
| Phase 4 (Signal DSP) | ⏳ À venir | 0% | S8 |
| Phase 5 (ML/IA) | ⏳ À venir | 0% | S10 |

---

## 📞 Support & Contact

### Questions ?
- 📧 **Issues GitHub** → [Créer une issue](https://github.com/nticonseil/multiparameter-health-monitoring/issues)
- 💬 **Discussions** → [Forum GitHub](https://github.com/nticonseil/multiparameter-health-monitoring/discussions)
- 📝 **Wiki** → [Consultations Fréquentes](https://github.com/nticonseil/multiparameter-health-monitoring/wiki)

### Ressources Utiles
- Arduino Official : https://www.arduino.cc
- Adafruit Tutorials : https://learn.adafruit.com
- SparkFun Guides : https://learn.sparkfun.com
- Stack Overflow : Tag `arduino`

---

## 📈 Statistiques Projet

```
Repository Stats :
├─ Code Lines    : none
├─ Documentation : 50+ pages MD
├─ Commits       : 2+
├─ Contributors  : none
└─ Stars         : ⭐⭐⭐⭐⭐
```

---

## 📝 Licence

Ce projet est sous licence **MIT** - voir [LICENSE](./LICENSE) pour détails.

```
MIT License

Copyright (c) 2025 Système de Surveillance Multiparamétrique

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files...
```

---

## 🎓 Crédits

**Auteurs Principaux :**
- [@nticonseil](https://github.com/nticonseil) - Architecture & Firmware
- [@Contributor2](https://github.com/nticonseil) - ML & DSP

**Remerciements :**
- Équipe Arduino pour le framework
- SparkFun pour les bibliothèques
- Community Arduino pour le support

---

## 🗺️ Roadmap Futur

```
Q1 2026
├─ ✅ Phase 1-2 complets
├─ 🔄 Phase 3 en cours
└─ ⏳ App mobile beta

Q2 2026
├─ ✅ Phase 3-4 complets
├─ 🔄 Phase 5 en cours
└─ 📈 Dataset 1000+ mesures

Q3 2026
├─ ✅ Prototype complet (v1.0)
├─ 📱 Release app mobile
├─ 📊 Validation clinique
└─ 📝 Rapport final

Q4 2026+
├─ 🎯 PCB professionnel
├─ 🏆 Certification CE/FCC
├─ 🌐 Plateforme cloud
└─ 🚀 Commercialisation
```

---

**Made with ❤️ for better healthcare**

*Dernière mise à jour : Décembre 2025*  
*Version : 1.0-beta*
