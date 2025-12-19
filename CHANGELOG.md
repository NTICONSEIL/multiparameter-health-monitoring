# 📝 CHANGELOG

Tous les changements notables de ce projet sont documentés dans ce fichier.

Format inspiré par [Keep a Changelog](https://keepachangelog.com/fr/) et respectant [Semantic Versioning](https://semver.org/lang/fr/).

---

## [Unreleased] - En Développement

### Ajouté (Added)
- Structure GitHub complète et documentation
- Phase 1 : Capteur MAX30102 fonctionnel
- Guide d'installation détaillé
- Cahier des charges complet
- Guide de contribution
- Architecture système documentée

### En Cours (In Progress)
- Phase 2 : Intégration MLX90614 + LCD I2C
- Tests unitaires Phase 1
- Code Arduino commenté Phase 1

### À Faire (Planned)
- Phase 3 : Bluetooth HC-05
- Phase 4 : Traitement signal avancé
- Phase 5 : Machine Learning TinyML
- Application mobile Android/iOS
- Dashboard web
- PCB professionnel

---

## [1.0-beta] - 2025-12-18

### Initialisation (Initial Release)

#### Ajouté
- ✅ Repository GitHub créé
- ✅ README.md avec vision projet
- ✅ Structure de dossiers complète
- ✅ Cahier des charges (v1.0)
- ✅ Guide installation Arduino IDE
- ✅ Configuration MAX30102
- ✅ Documentation I2C
- ✅ Premiers sketches Arduino (templates)

#### Documenté
- ✅ Architecture système globale
- ✅ Planning 10 semaines
- ✅ Budget et composants
- ✅ Phases développement
- ✅ Critères d'acceptation
- ✅ Risques et mitigations

#### Configuré
- ✅ .gitignore pour Arduino
- ✅ GitHub Actions (optionnel)
- ✅ Issue templates
- ✅ Pull Request template
- ✅ Licences MIT

---

## Versions Futures Prévues

### v0.1-phase1 (Semaine 2)
- Lecture MAX30102 validée
- Tests unitaires Phase 1
- Documentation Phase 1 complète

### v0.2-phase2 (Semaine 4)
- MLX90614 intégré
- LCD I2C fonctionnel
- 3 paramètres affichés simultanément
- Tests d'intégration

### v0.3-phase3 (Semaine 6)
- HC-05 Bluetooth operationnel
- App mobile basique
- Transmission temps réel

### v0.5-phase4 (Semaine 8)
- Filtres numériques implémentés
- Stockage microSD
- Analyse spectrale FFT
- Dashboard web prototype

### v1.0-final (Semaine 10)
- TinyML intégré
- Modèle ML entraîné
- Tests validation clinique
- Rapport final complet
- Version stable production-ready

---

## Notes Développement

### Convention de Versioning

```
MAJEUR.MINEUR-PHASE

Exemple: 1.0-beta (beta = pré-release)
         0.1-alpha (alpha = early development)
         1.0      (stable release)
```

### Tags Git

```
v1.0-beta           Phase complète 1-2
v0.1-phase1         Phase 1 uniquement
v0.2-phase2         Phase 2 complète
rc1, rc2, rc3       Release Candidates
```

---

## Changements Historiques Détaillés

### 2025-12-18 - Session Initiale

**Contexte :** Démarrage projet BTS Santé Connectée

**Actions :**
1. Création repository GitHub
2. Mise en place structure complète
3. Documentation initiale (45 pages)
4. Planning 10 semaines
5. Cahier des charges professionnel

**Fichiers Créés :**
- README.md (40 KB)
- INSTALLATION_GUIDE.md (25 KB)
- CONTRIBUTING.md (18 KB)
- CHANGELOG.md (ce fichier)
- .gitignore
- Structure dossiers 30+

**Status :** ✅ Prêt pour Phase 1

---

## Guide Lecture CHANGELOG

Pour chaque version, vous trouverez :

### 🆕 Ajouté (Added)
Nouvelles fonctionnalités, nouveaux fichiers

### ✏️ Modifié (Changed)
Changements à la fonctionnalité existante

### 🔨 Déprécié (Deprecated)
Fonctionnalité en fin de vie (toujours fonctionnelle)

### ❌ Supprimé (Removed)
Fonctionnalité/fichiers supprimés

### 🔧 Corrigé (Fixed)
Corrections bugs

### 🚨 Sécurité (Security)
Patches sécurité

---

## Roadmap Produit

```
Q4 2025 (Décembre-Janvier)
├─ Phase 1-2 complètes
├─ MVP fonctionnel
├─ Documentation complète
└─ v0.2-phase2 released

Q1 2026 (Février-Mars)
├─ Phase 3-4 complètes
├─ Bluetooth et DSP
├─ Tests validation
└─ v0.5-phase4 released

Q2 2026 (Avril-Mai)
├─ Phase 5 complète
├─ Machine Learning
├─ Validation clinique
└─ v1.0 released

Q3 2026+ (Juin+)
├─ PCB professionnel
├─ Certification CE/FCC
├─ Plateforme cloud
└─ v2.0 commerciale
```

---

## Contribution History

| Date | Contributeur | Type | Commits | PR |
|------|--------------|------|---------|-----|
| 2025-12-18 | @MainAuthor | init | 1 | #1 |
| (à remplir) | @Contributor1 | feat | ? | ? |
| (à remplir) | @Contributor2 | fix | ? | ? |

---

## Crédits

### Mainteneurs
- 👤 [@YourName](https://github.com/nticonseil) - Architecture & Lead
- 👤 [@Co-Maintainer](https://github.com/nticonseil) - Reviews & QA

### Contributeurs
*À remplir au fur et à mesure*

### Remerciements
- Arduino Community
- SparkFun pour les bibliothèques
- Adafruit pour les tutoriels
- Tous les testeurs et reviewers

---

## Comment Reporter les Changements

Lors de la création d'une Pull Request, **mettez à jour CHANGELOG.md** :

```markdown
### [Version] - YYYY-MM-DD

#### Ajouté
- Description du changement 1
- Description du changement 2

#### Corrigé
- Correction du bug X
```

**Format date :** [ISO 8601](https://fr.wikipedia.org/wiki/ISO_8601) : `YYYY-MM-DD`

---

## Changelog Complet

Pour accéder au **changelog complet** :
- Voir [Releases GitHub](https://github.com/nticonseil/multiparameter-health-monitoring/releases)
- Voir [Git History](https://github.com/nticonseil/multiparameter-health-monitoring/commits/main)
- Voir [Compare Versions](https://github.com/nticonseil/multiparameter-health-monitoring/compare)

---

**Document :** CHANGELOG.md  
**Version :** 1.0  
**Dernier Update :** 19 Décembre 2025  
**Licence :** MIT

---

*Ce CHANGELOG sera mis à jour régulièrement au fur et à mesure de la progression du projet.*
