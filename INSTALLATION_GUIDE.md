# 📋 Guide d'Installation Détaillé

**Date :** Décembre 2025  
**Version :** 1.0  
**Temps estimé :** 45 minutes

---

## Table des Matières

1. [Prérequis](#-prérequis)
2. [Installation Arduino IDE](#-installation-arduino-ide)
3. [Préparation de la Carte Arduino](#-préparation-de-la-carte-arduino)
4. [Installation des Bibliothèques](#-installation-des-bibliothèques)
5. [Configuration des Ports](#-configuration-des-ports)
6. [Test de Connexion](#-test-de-connexion)
7. [Dépannage](#-dépannage)

---

## 🔧 Prérequis

### Matériel Minimum (Phase 1)

- ✅ Arduino Uno R3 (original ou clone)
- ✅ Capteur MAX30102
- ✅ Câble USB Type-B (pour Arduino)
- ✅ Câbles Dupont mâle-femelle (10+)
- ✅ Breadboard 830 points
- ✅ Ordinateur (Windows, macOS, Linux)

### Logiciels

- ✅ Arduino IDE 2.0 ou supérieur
- ✅ Pilotes USB (généralement auto-installés)
- ✅ Git (optionnel mais recommandé)

### Compétences

- Lecture schémas électriques basiques
- Manipulation petits composants électroniques
- Connaissances Arduino élémentaires

---

## 📲 Installation Arduino IDE

### Windows

1. **Télécharger** depuis https://www.arduino.cc/en/software
2. **Exécuter** l'installateur `.exe`
3. **Suivre** l'assistant (accepter tous les chemins par défaut)
4. **Terminer** l'installation
5. **Lancer** Arduino IDE depuis le menu Démarrer

### macOS

```bash
# Via Homebrew (recommandé)
brew install arduino-ide

# Ou télécharger depuis https://www.arduino.cc/en/software
```

### Linux (Ubuntu/Debian)

```bash
# Installation via snap (recommandé)
sudo snap install arduino

# Ou via apt
sudo apt update
sudo apt install arduino
```

**Vérification :** Ouvrir Arduino IDE → Menu `Help` → `About Arduino IDE` doit afficher version 2.0+

---

## 🔌 Préparation de la Carte Arduino

### Vérifier la Carte

1. **Connecter** Arduino via câble USB
2. **Ouvrir** Arduino IDE
3. **Aller à** `Tools` → `Board` → Sélectionner **"Arduino Uno"**
4. **Aller à** `Tools` → `Port`
   - **Windows :** Doit voir `COM3` (ou autre) avec "Arduino Uno"
   - **macOS :** `/dev/cu.usbserial-*`
   - **Linux :** `/dev/ttyUSB0` ou `/dev/ttyACM0`

### Test de Connexion Basique

1. **File** → **Examples** → **01.Basics** → **Blink**
2. **Sketch** → **Verify** (Vérifier - compiler)
   - Doit afficher ✅ "Compilation successful"
3. **Sketch** → **Upload** (Téléverser)
   - LED orange sur Arduino doit clignoter rapidement
   - Message : ✅ "Done uploading"

---

## 📚 Installation des Bibliothèques

### Méthode 1 : Via Arduino Library Manager (Recommandée)

#### Bibliothèque MAX30102

```
Sketch → Include Library → Manage Libraries...
Chercher : "SparkFun MAX3010x"
Cliquer : Install (v1.1.1 ou plus récent)
```

#### Bibliothèque MLX90614

```
Sketch → Include Library → Manage Libraries...
Chercher : "Adafruit MLX90614"
Cliquer : Install
```

#### Bibliothèque LCD I2C

```
Sketch → Include Library → Manage Libraries...
Chercher : "LiquidCrystal I2C"
Auteur : Frank de Brabander
Cliquer : Install
```

**Vérification :** Dans Arduino IDE, `Sketch` → `Include Library` doit montrer les 3 bibliothèques.

### Méthode 2 : Installation Manuelle (Si problème)

```bash
# Windows
cd %APPDATA%\Arduino\libraries

# macOS
cd ~/Documents/Arduino/libraries

# Linux
cd ~/Arduino/libraries

# Cloner les repos
git clone https://github.com/sparkfun/SparkFun_MAX3010x_Sensor_Library.git
git clone https://github.com/adafruit/Adafruit_MLX90614.git
git clone https://github.com/marcoschwartz/LiquidCrystal_I2C.git
```

---

## 🔌 Configuration des Ports

### Câblage MAX30102

| MAX30102 | Arduino Uno | Couleur |
|----------|-------------|---------|
| VCC | 3.3V | Rouge |
| GND | GND | Noir |
| SDA | A4 | Vert |
| SCL | A5 | Jaune |

**⚠️ Important :** MAX30102 fonctionne à 3.3V, PAS 5V !

### Câblage LCD I2C

| LCD I2C | Arduino Uno | Couleur |
|---------|-------------|---------|
| VCC | 5V | Rouge |
| GND | GND | Noir |
| SDA | A4 | Vert |
| SCL | A5 | Jaune |

**Note :** LCD et MAX30102 partagent le même bus I2C (A4/A5)

### Photo Breadboard

```
                    Breadboard 830
┌─────────────────────────────────────────┐
│ +5V━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓   │
│ GND━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛   │
│                                        │
│  [MAX30102]     [LCD I2C]             │
│  VCC→+3.3V      VCC→+5V               │
│  GND→GND        GND→GND               │
│  SDA→A4         SDA→A4 (même fil)     │
│  SCL→A5         SCL→A5 (même fil)     │
│                                        │
│  ┌──────────────────────────────┐     │
│  │ Arduino Uno R3               │     │
│  │ ┌──────────────────────────┐ │     │
│  │ │ VCC  GND  5V  3.3V  RST  │ │     │
│  │ │ A0   A1  A2  A3  A4  A5  │ │     │
│  │ │  0    1   2   3   4   5  │ │     │
│  │ │  6    7   8   9  10  11  │ │     │
│  │ │ 12   13  GND 5V  Vin    │ │     │
│  │ └──────────────────────────┘ │     │
│  └──────────────────────────────┘     │
│                                        │
└─────────────────────────────────────────┘
```

---

## 🧪 Test de Connexion

### Test 1 : Vérifier I2C

Créer un sketch de test :

```cpp
#include <Wire.h>

void setup() {
  Serial.begin(9600);
  Wire.begin();
  delay(2000);
  
  Serial.println("Scanning I2C addresses...");
  
  int count = 0;
  for (int i = 1; i < 127; i++) {
    Wire.beginTransmission(i);
    if (Wire.endTransmission() == 0) {
      Serial.print("Found device at address 0x");
      Serial.println(i, HEX);
      count++;
    }
  }
  
  Serial.print("Found ");
  Serial.print(count);
  Serial.println(" device(s)");
}

void loop() {}
```

**Résultats attendus :**

```
Scanning I2C addresses...
Found device at address 0x57      // MAX30102
Found device at address 0x5A      // MLX90614
Found device at address 0x27      // LCD I2C
Found 3 device(s)
```

### Test 2 : Mesure Basique MAX30102

```cpp
#include "MAX30105.h"

MAX30105 particleSensor;

void setup() {
  Serial.begin(115200);
  
  if (!particleSensor.begin()) {
    Serial.println("MAX30102 not found!");
    while (1);
  }
  
  Serial.println("MAX30102 initialized!");
  
  particleSensor.setup();
  particleSensor.setPulseAmplitudeRed(0x0A);
  particleSensor.setPulseAmplitudeIR(0x0A);
}

void loop() {
  uint32_t irValue = particleSensor.getIR();
  Serial.print("IR Value: ");
  Serial.println(irValue);
  delay(1000);
}
```

**Résultats attendus :** Valeurs IR > 50000 quand doigt sur capteur

---

## 🆘 Dépannage

### Arduino IDE ne reconnaît pas la carte

**Symptôme :** Port vide ou absent dans `Tools → Port`

**Solutions :**

1. **Vérifier câble USB**
   - Essayer un autre câble
   - Tester sur autre ordinateur
   
2. **Réinstaller pilotes**
   - Windows : https://www.arduino.cc/en/Guide/Windows
   - macOS : Généralement auto
   - Linux : `sudo usermod -a -G dialout $USER` puis redémarrer

3. **Réinitialiser Arduino**
   - Maintenir appuyé le bouton `RESET` 2 secondes
   - Relâcher et attendre 3 secondes
   - Tenter nouveau port

### I2C ne détecte pas les capteurs

**Symptôme :** Scanner I2C affiche "Found 0 device(s)"

**Solutions :**

1. **Vérifier câblage**
   ```
   ☐ VCC correctement connecté (3.3V pour MAX, 5V pour LCD)
   ☐ GND relié
   ☐ SDA/SCL sur bonnes broches (A4/A5)
   ☐ Pas de court-circuit
   ```

2. **Ajouter pullup résistances**
   - Ajouter 2× résistance 4.7kΩ entre :
     - 5V → SDA
     - 5V → SCL
   
3. **Tester chaque capteur isolément**
   - Débrancher LCD
   - Vérifier MAX30102 seul
   - Puis ajouter LCD

### Erreur compilation "LiquidCrystal_I2C not found"

**Solutions :**

1. Vérifier installation dans Library Manager
2. Redémarrer Arduino IDE
3. Inclure correct : `#include <LiquidCrystal_I2C.h>`

### MAX30102 donne des valeurs bizarres

**Symptôme :** BPM = 0 ou valeurs aberrantes

**Solutions :**

1. **Calibration :** Placer doigt 1-2 cm sur capteur LED
2. **Patience :** Attendre 3-5 secondes pour stabilisation
3. **Nettoyage :** Essuyer lentille LED avec tissu doux
4. **Tension :** Vérifier 3.3V stable sur MAX30102

### LCD I2C affiche rien

**Symptôme :** Écran noir ou non-réactif

**Solutions :**

1. **Potentiomètre de contraste**
   - Tourner petite vis avec tournevis sur dos LCD
   - Régler jusqu'à voir caractères

2. **Vérifier adresse I2C**
   - Par défaut : 0x27
   - Vérifier avec scanner I2C
   - Ajuster dans code : `LiquidCrystal_I2C lcd(0x27, 16, 2);`

3. **Backlight LED**
   - Vérifier si LED s'allume
   - Tester alimentation LCD I2C

---

## ✅ Checklist Finale

Avant de commencer le projet :

- [ ] Arduino IDE 2.0+ installé
- [ ] Arduino Uno reconnu dans `Tools → Port`
- [ ] Blink.ino compile et upload ✅
- [ ] Bibliothèque SparkFun MAX3010x installée
- [ ] Bibliothèque Adafruit MLX90614 installée
- [ ] Bibliothèque LiquidCrystal_I2C installée
- [ ] MAX30102 câblé (VCC 3.3V, GND, A4, A5)
- [ ] LCD I2C câblé (VCC 5V, GND, A4, A5)
- [ ] I2C Scanner détecte 2 devices (0x57, 0x5A ou 0x27)
- [ ] Serial Monitor teste 9600 baud

**Prêt ?** Passer à [Phase 1](../firmware/Phase1_MAX30102_basic/README.md) ! 🚀

---

**Document créé :** Décembre 2025  
**Auteur :** Équipe Projet BTS  
**Licence :** MIT
