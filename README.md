# Prédiction de l’Attrition des Conducteurs (Driver Turnover)

**Projet Data Science | Python | Analyse de survie & Machine Learning**

Pipeline complet de data engineering et de modélisation prédictive pour anticiper le départ des conducteurs à partir de données RH hétérogènes.  
Le projet combine **analyse de survie** et **modèles de classification** (Logistic Regression, Random Forest, XGBoost, LightGBM, MLP) afin d’identifier les facteurs de risque et de fournir un outil d’aide à la décision.

---

## Contexte & Objectifs

Les entreprises de transport font face à un fort turnover des conducteurs, ce qui génère des coûts importants de recrutement et de formation.  

**Objectifs du projet :**
- Construire une base de données unifiée à partir de multiples sources RH
- Analyser les durées d’emploi et les facteurs de départ (analyse de survie)
- Développer des modèles prédictifs d’attrition
- Comparer plusieurs algorithmes et sélectionner le plus performant
- Fournir des recommandations actionnables pour la rétention

---

## Données

Le projet intègre et nettoie **plus de 15 tables** provenant de systèmes RH et opérationnels :

| Table                        | Description                              | Volume approximatif |
|-----------------------------|------------------------------------------|---------------------|
| terms.csv                   | Terminations                             | 6 500 lignes        |
| staffing_file.csv           | Fichier de personnel                     | 459 000 lignes      |
| wages.csv                   | Salaires hebdomadaires                   | 1 060 000 lignes    |
| discipline_record.csv       | Sanctions disciplinaires                 | 3 200 lignes        |
| accident_registry.csv       | Accidents                                | 9 800 lignes        |
| roadside_inspections_report | Contrôles routiers                       | 5 700 lignes        |
| watchlist_score_history     | Scores de surveillance                   | 500 000 lignes      |
| raw_load_quality_survey     | Enquêtes de satisfaction                 | 555 000 lignes      |
| ...                         | Autres tables (VOE, TOC, compensation…)  | -                   |

**Traitements effectués :**
- Nettoyage et standardisation des noms de colonnes
- Suppression des doublons exacts
- Gestion des dates et de la censure (date de fin d’observation : 26 juin 2026)
- Construction d’une table unique au niveau conducteur (`driver_id`)

---

## Méthodologie

### 1. Analyse de survie
- Courbes de Kaplan-Meier
- Tests du log-rank (univarié et multivarié)
- Identification des facteurs influençant la durée d’emploi

### 2. Feature Engineering
- Variables numériques (ancienneté, salaires, nombre d’accidents, sanctions, etc.)
- Variables catégorielles (site, type de contrat, etc.)
- Construction de la variable cible d’attrition (`Y`)

### 3. Modélisation prédictive
Modèles comparés avec validation croisée stratifiée (5 folds) :

| Modèle          | Type                     |
|-----------------|--------------------------|
| Logistic Regression | Linéaire + class_weight |
| Random Forest   | Ensemble d’arbres        |
| XGBoost         | Gradient Boosting        |
| **LightGBM**    | Gradient Boosting (meilleur) |
| MLP             | Réseau de neurones       |

**Métriques utilisées :** AUC-ROC, Average Precision, Log-Loss, Brier Score, Precision, Recall, F1-Score

**Meilleur modèle :** **LightGBM** (meilleure AUC moyenne en validation croisée)

---

## Structure du projet

```text
driver-attrition-prediction/
├── notebook/
│   └── Traitement_complet_EN.ipynb
├── data/                    # (non inclus – données confidentielles)
├── outputs/
│   └── figures/
├── README.md
└── requirements.txt
---
Structure du projet
```text
driver-attrition-prediction/
├── notebook/
│   └── Traitement_complet_EN.ipynb
├── data/                    # (non inclus – données confidentielles)
├── outputs/
│   └── figures/
├── README.md
└── requirements.txt
```
---
Technologies utilisées
Python 3.11
pandas, numpy
matplotlib, seaborn
lifelines (analyse de survie)
scikit-learn
XGBoost
LightGBM
scipy
---
Installation
```bash
pip install -r requirements.txt
```
Contenu recommandé de `requirements.txt` :
```txt
pandas
numpy
matplotlib
seaborn
scipy
lifelines
scikit-learn
xgboost
lightgbm
openpyxl
jupyter
```
---
**Recommandations clés**
- Utiliser le modèle LightGBM pour le scoring des conducteurs à risque  
- Mettre en place un suivi mensuel des variables les plus influentes (ancienneté, historique disciplinaire, accidents, satisfaction)  
- Combiner les scores prédictifs avec les courbes de survie pour prioriser les actions de rétention  
- Réentraîner régulièrement le modèle avec les nouvelles données de départ  
- Développer un tableau de bord simple pour les équipes RH  
---
Auteur
Bebou Bienvenu YO  
Analyste Statisticien | Data Science  
ISSP – Université Joseph Ki-Zerbo, Ouagadougou, Burkina Faso
Email : bebouyo@gmail.com
---
Notes
Les données utilisées dans ce projet sont confidentielles et ne sont pas fournies dans le dépôt.  
Le notebook contient l’intégralité du pipeline de traitement, d’analyse de survie et de modélisation.
```
