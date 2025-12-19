# 🤝 Guide de Contribution

Merci de ton intérêt pour ce projet ! Ce guide explique comment **contribuer efficacement**.

---

## 📋 Table des Matières

1. [Code de Conduite](#-code-de-conduite)
2. [Comment Contribuer](#-comment-contribuer)
3. [Processus Pull Request](#-processus-pull-request)
4. [Standards de Code](#-standards-de-code)
5. [Tests et Validation](#-tests-et-validation)
6. [Documentation](#-documentation)

---

## 🎯 Code de Conduite

Tous les contributeurs doivent respecter ce code :

- ✅ **Respectueux** : Traitez tout le monde avec bienveillance
- ✅ **Constructif** : Les critiques doivent être utiles
- ✅ **Inclusif** : Bienvenue à tous les niveaux d'expérience
- ✅ **Transparent** : Communiquez clairement vos intentions

**Violations ?** Contactez les mainteneurs directement.

---

## 💡 Comment Contribuer

### Types de Contributions Bienvenues

```
📝 Documentation
├─ Corrections typos
├─ Clarifications
├─ Traductions
└─ Nouveaux tutoriels

🐛 Bug Fixes
├─ Reproductibilité confirmée
├─ Causé identification
├─ Solution proposée
└─ Tests inclus

✨ Nouvelles Features
├─ Issue discutée d'abord
├─ Design approuvé
├─ Tests complets
└─ Doc mise à jour

🧪 Tests et Validation
├─ Test unitaires
├─ Test d'intégration
├─ Données de référence
└─ Rapport précision
```

### Avant de Démarrer

1. **Vérifier les issues existantes**
   - Issue fermée ? → Ne pas dupliquer
   - Issue ouverte ? → Vous pouvez commencer

2. **Pour Bug Significatif**
   - Créer une issue d'abord
   - Discuter approche avec mainteneurs
   - Attendre approbation

3. **Pour Petite Correction**
   - Pas besoin de demander
   - Créer branche et PR directement

---

## 🔄 Processus Pull Request

### Étape 1 : Fork & Clone

```bash
# Fork sur GitHub (bouton en haut à droite)
git clone https://github.com/nticonseil/multiparameter-health-monitoring.git
cd multiparameter-health-monitoring
```

### Étape 2 : Créer Branche

```bash
# Depuis develop (pas main)
git checkout -b feature/ma-feature
# OU
git checkout -b fix/mon-bug
# OU
git checkout -b docs/clarifications
```

**Convention branches :**
```
feature/*    = Nouvelle fonctionnalité
fix/*        = Correction bug
docs/*       = Mise à jour documentation
refactor/*   = Refactorisation code
test/*       = Ajout tests
```

### Étape 3 : Développer & Commit

```bash
# Apporter changements
git add .

# Commits clairs et structurés
git commit -m "feat: Add I2C scanner utility

- Add diagnostic tool for I2C bus
- Display device addresses in hex
- Include timeout handling

Closes #42"
```

**Format commit :**
```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

Types : `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Étape 4 : Push & PR

```bash
git push origin feature/ma-feature
```

Aller sur GitHub → Créer **Pull Request** :

```markdown
## Description
Courte description des changements

## Type de Changement
- [ ] Bug fix (non-breaking)
- [ ] Nouvelle feature (non-breaking)
- [ ] Breaking change
- [ ] Documentation update

## Checklist
- [ ] J'ai suivi le style guide
- [ ] J'ai testé ma contribution
- [ ] J'ai mis à jour la documentation
- [ ] J'ai ajouté des tests

## Tests Effectués
Décrire les tests réalisés

## Screenshots (optionnel)
Ajouter images si applicable
```

### Étape 5 : Répondre aux Reviews

1. **Reviews reçues ?** → Répondre à chaque commentaire
2. **Changements demandés ?** → Apporter corrections + push
3. **Conflict ?** → Résoudre en updateant branche
4. **Approbation ?** → Attendre merge maintainers

---

## 📐 Standards de Code

### Arduino/C++ Style

```cpp
// Noms significatifs
int sensorReadingValue = 0;              // BON
int srVal = 0;                           // MAUVAIS

// Indentation : 2 espaces
if (condition) {
  doSomething();
}

// Constantes en MAJUSCULES
#define MAX_READINGS 100
const int I2C_ADDRESS_SENSOR = 0x57;

// Commentaires clairs
// Read raw ADC value from sensor
int rawValue = analogRead(PIN_SENSOR);

// Fonctions avec documentation
/**
 * Calculate average of last N readings
 * @param values Array of readings
 * @param count Number of readings
 * @return Average value
 */
int calculateAverage(int* values, int count) {
  int sum = 0;
  for (int i = 0; i < count; i++) {
    sum += values[i];
  }
  return sum / count;
}
```

### Python Style

```python
# Suivre PEP 8
# - Lines < 79 characters
# - snake_case for functions/variables
# - UPPERCASE for constants

def calculate_moving_average(data, window_size):
    """
    Calculate moving average
    
    Args:
        data: List of values
        window_size: Size of moving window
        
    Returns:
        List of averaged values
    """
    if window_size > len(data):
        raise ValueError("Window size exceeds data length")
    
    result = []
    for i in range(len(data) - window_size + 1):
        window = data[i:i + window_size]
        result.append(sum(window) / window_size)
    return result
```

### Fichiers README par Dossier

Chaque dossier important doit avoir README expliquant :
- 📝 But/contenu du dossier
- 🚀 Comment utiliser
- 📚 Références/liens

---

## 🧪 Tests et Validation

### Avant de Soumettre PR

```bash
# ✅ Code compile sans erreurs
arduino-cli compile --fqbn arduino:avr:uno firmware/Phase1_MAX30102_basic/

# ✅ Tests unitaires passent (si applicable)
make test

# ✅ Pas d'avertissements Lint
cpplint firmware/**/*.cpp
```

### Tests Requis par Type

| Type | Tests Requis |
|------|-------------|
| **Bug Fix** | Test reproduisant le bug |
| **Feature** | Tests unitaires + intégration |
| **Docs** | Relecture orthographe/grammaire |
| **Refactor** | Tous les tests existants doivent passer |

### Exemple Test Unitaire

```cpp
// tests/test_Filters.cpp
#include <unity.h>
#include "../firmware/Filters.h"

void test_moving_average_basic() {
    int data[] = {10, 20, 30, 40, 50};
    int result = calculateMovingAverage(data, 5);
    TEST_ASSERT_EQUAL_INT(30, result);  // Average = 30
}

void test_moving_average_empty() {
    int data[] = {};
    TEST_ASSERT_EQUAL_INT(-1, calculateMovingAverage(data, 0));
}

void setup() {
    UNITY_BEGIN();
    RUN_TEST(test_moving_average_basic);
    RUN_TEST(test_moving_average_empty);
    UNITY_END();
}

void loop() {}
```

---

## 📚 Documentation

### Checklist Documentation

Pour chaque PR :

- [ ] Commentaires dans le code (pourquoi, pas quoi)
- [ ] Docstrings pour fonctions publiques
- [ ] README mis à jour si changement fonctionnel
- [ ] CHANGELOG.md mis à jour
- [ ] Exemple d'utilisation si nouvelle feature

### Format Documentation

```markdown
## Feature X

### Description
Explication claire de la feature

### Utilisation
```cpp
// Exemple de code
```

### Limitations
- Limitation 1
- Limitation 2

### Références
- [Lien 1]()
- [Lien 2]()
```

---

## 🏆 Bonnes Pratiques

✅ **À Faire**

- Créer PR petites et focalisées
- Un changement logique = une PR
- Tester avant soumettre
- Écrire messages descriptifs
- Répondre aux reviews rapidement
- Utiliser issue templates

❌ **À Éviter**

- PRs géantes (>500 lignes)
- Mélanger features/fixes/docs
- Commits avec 10 fichiers différents
- Messages peu clairs ("fix bug")
- Ignorer les reviews
- Force-push sur PR déjà reviewée

---

## 🎁 Premiers Pas (Beginner-Friendly)

Si c'est votre **première contribution**, cherchez issues avec label :

```
good first issue
help wanted
documentation
```

Ces issues sont explicitement conçues pour débuter !

---

## 📞 Questions ?

- 📧 Créer une issue → "Question: ..."
- 💬 Utiliser Discussions pour conversations
- 👥 Contacter mainteneurs directement si bloqué

---

## 🙏 Merci !

Votre contribution aide à améliorer ce projet et à bénéficier à toute la communauté.

**On vous attend ! 🚀**

---

**Document :** CONTRIBUTING.md  
**Version :** 1.0  
**Dernier Update :** Décembre 2025
