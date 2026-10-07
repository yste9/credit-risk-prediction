# credit-risk-prediction
Prédiction du risque de défaut de crédit avec un réseau de neurones MLP : préparation des données, classification binaire et évaluation pour l’aide à la décision de crédit.

# Prédiction du risque de défaut de crédit

**Modélisation par réseau de neurones sur le Credit Risk Dataset.**

Projet universitaire réalisé par **Yannick ASSI**, dans le cadre du cours **Data Management & Data Mining**.

📅 Février 2026

## 1. 🎯 Présentation de l’étude

### Problématique

Comment exploiter les données d’un emprunteur et de son prêt pour prédire le risque de défaut et soutenir la décision de crédit ?

Ce projet étudie un problème de **classification supervisée binaire** à partir de la variable cible `loan_status` :

- `0` : absence de défaut ;
- `1` : défaut.

### Objectifs

- Structurer et préparer les données.
- Construire un modèle de prédiction du défaut.
- Évaluer ses performances sur des données non utilisées pour l’apprentissage.
- Interpréter les résultats dans une perspective d’aide à la décision.

### Données

Le projet utilise le **Credit Risk Dataset**, disponible sur Kaggle :

[Consulter le jeu de données](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)

Le jeu de données initial comporte **32 581 observations et 12 variables**.

| Catégorie | Informations utilisées |
|---|---|
| Profil de l’emprunteur | Âge, revenu, situation résidentielle, ancienneté professionnelle |
| Caractéristiques du prêt | Montant, taux et autres caractéristiques du crédit |
| Historique de crédit | Informations relatives aux antécédents de crédit |
| Variable cible | `loan_status` |

### Démarche de l’étude

1. Explorer les données et identifier les valeurs manquantes.
2. Préparer les variables numériques et catégorielles.
3. Séparer les données en ensembles d’apprentissage et de test.
4. Construire un réseau de neurones multi-perceptron.
5. Entraîner le modèle.
6. Produire un score pour chaque observation du jeu de test.
7. Transformer les scores en classes selon un seuil.
8. Évaluer les prédictions et discuter leur intérêt métier.

## 2. ⚙️ Préparation des données réalisée

Les traitements présentés dans l’étude sont :

- Imputation par la moyenne des valeurs manquantes, notamment pour :
  - `person_emp_length` ;
  - `loan_int_rate`.
- Encodage binaire des variables catégorielles.
- Séparation des données en deux ensembles.

| Ensemble | Proportion | Nombre d’observations |
|---|---:|---:|
| Apprentissage | 80 % | 26 065 |
| Test | 20 % | 6 516 |

Pour reproduire l’étude, les paramètres de préparation doivent être documentés dans le code. Les transformations estimées à partir des données, comme les moyennes d’imputation, doivent être calculées sur l’ensemble d’apprentissage uniquement.

## 3. 🧠 Modèle utilisé

Le modèle retenu est un **réseau de neurones multi-perceptron — MLP**.

### Architecture

| Couche | Nombre de neurones |
|---|---:|
| Entrée | 10 |
| Couche cachée | 4 |
| Sortie | 1 |

**Architecture globale : 10 → 4 → 1**

Le réseau utilise une fonction d’activation **sigmoïde**.

La sortie est un score compris entre `0` et `1`, interprété dans l’étude comme une estimation du risque de défaut.

Un seuil permet de convertir ce score en prédiction :

- Score inférieur au seuil : classe « non-défaut ».
- Score supérieur ou égal au seuil : classe « défaut ».

La calibration de ce score doit être évaluée avant de le considérer comme une probabilité fiable.

### Apprentissage

| Paramètre | Configuration rapportée |
|---|---|
| Algorithme | Resilient Backpropagation — RProp |
| Nombre maximal d’itérations | 150 |
| Régularisation | Elastic Net, paramètre 0,01 |
| Erreur suivie | Sum of Squared Errors — SSE |

## 4. 📈 Résultats obtenus

### Évolution de l’erreur d’apprentissage

| Indicateur | Valeur rapportée |
|---|---:|
| SSE initiale | 1 628 |
| SSE finale | 719 |

La diminution de l’erreur montre que le modèle améliore son ajustement aux données d’apprentissage.

Elle ne suffit pas à démontrer sa capacité de généralisation, qui doit être appréciée sur le jeu de test.

### Performance sur le jeu de test

La présentation rapporte environ **86 % de dossiers correctement classés**.

| Métrique | Résultat |
|---|---:|
| Accuracy sur le jeu de test | ≈ 86 % |

Cette valeur décrit la performance globale au seuil utilisé dans l’étude. Le seuil exact n’est pas précisé dans la présentation.

Une courbe ROC est également présentée pour examiner le pouvoir discriminant du modèle. Aucune valeur numérique d’AUC n’est rapportée dans le document.

### Interprétation

L’accuracy constitue un premier indicateur, mais elle ne suffit pas à déterminer si le modèle détecte correctement les emprunteurs en défaut.

L’évaluation doit distinguer :

- **Faux négatifs** : emprunteurs en défaut classés comme non-défaillants.
- **Faux positifs** : emprunteurs non-défaillants classés comme défaillants.

Ces erreurs ont des conséquences différentes pour la décision de crédit.

## 5. 💼 Intérêt métier

Le modèle explore la possibilité de :

- Repérer les dossiers présentant un risque de défaut.
- Produire un score pour aider à hiérarchiser les dossiers à examiner.
- Adapter le seuil de classification selon les objectifs de détection.
- Soutenir l’analyse du risque au sein d’un portefeuille.

Ces usages constituent des perspectives d’application. Le projet ne démontre pas une réduction effective des pertes ni une amélioration opérationnelle de l’octroi de crédit.

## 6. 🔎 Limites et perspectives

### Limites

- L’accuracy seule ne décrit pas les performances de chaque classe.
- La présentation ne précise pas le seuil de classification.
- Les valeurs de précision, rappel, F1-score et AUC ne sont pas rapportées.
- La calibration des scores n’est pas documentée.
- Les résultats doivent être comparés à une méthode de référence.
- Les paramètres complets et les étapes de préparation doivent être vérifiés dans le code.

### Pistes d’amélioration

- Ajouter une matrice de confusion.
- Calculer la précision, le rappel et le F1-score pour la classe « défaut ».
- Mesurer l’AUC ROC et examiner la courbe précision-rappel.
- Comparer le MLP à une régression logistique et à des modèles d’arbres.
- Optimiser les hyperparamètres sur un ensemble de validation.
- Étudier l’effet du seuil sur les erreurs de classification.
- Évaluer la calibration et l’explicabilité du modèle.
- Documenter les dépendances et la graine aléatoire pour faciliter la reproduction.

## 7. 🛠️ Compétences mobilisées

- Exploration et préparation de données.
- Traitement des valeurs manquantes.
- Encodage de variables catégorielles.
- Classification supervisée binaire.
- Réseaux de neurones multi-perceptrons.
- Évaluation de modèles prédictifs.
- Analyse du risque de crédit.
- Interprétation des résultats pour l’aide à la décision.

## 8. 👤 Auteur

**Yannick ASSI**

Master MIASHS - Université Catholique de l’Ouest  
Projet de Data Management & Data Mining
