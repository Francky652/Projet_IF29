# IF29 — Classification de profils X (Twitter) atypiques

> Projet académique — **UE IF29 : Traitement de données (Data Analytics)**
> Université de Technologie de Troyes (UTT)
> Année universitaire 2025-2026

Comparaison de deux approches de Machine Learning — **supervisée** et **non supervisée** — pour la détection de profils suspects, automatisés ou atypiques sur la plateforme X (anciennement Twitter), à partir du jeu de données `Tweet_Worldcup`.

---
## Introduction du projet

Ce projet présente le travail réalisé par le groupe 2 dans le cadre de l’UE IF29 — Traitement de données (Data Analytics). Il porte sur la détection de profils atypiques sur X, anciennement Twitter, à partir du jeu de données Tweet_Worldcup.

L’objectif principal est de mettre en œuvre et de comparer deux approches de Machine Learning appliquées à une même problématique et à un même jeu de données : une approche non supervisée et une approche supervisée. L’approche non supervisée, basée sur le clustering, permet d’explorer les comportements des utilisateurs sans utiliser de labels prédéfinis. L’approche supervisée, basée sur un modèle de classification, vise à identifier automatiquement des profils considérés comme atypiques selon des critères définis à partir des données.

Dans ce projet, un profil atypique désigne un utilisateur dont les caractéristiques statistiques ou comportementales s’écartent fortement de celles de la majorité des comptes observés. Il peut par exemple s’agir d’un compte ayant une activité anormalement élevée, une faible visibilité malgré de nombreuses publications, ou une utilisation importante de hashtags et de liens externes. Les profils détectés doivent cependant être considérés comme potentiellement atypiques, et non comme des bots ou des spammeurs certifiés.

Ce dépôt contient les notebooks, les scripts, les données intermédiaires et les ressources nécessaires à la préparation des données, à l’entraînement des modèles, à l’analyse des résultats et à la comparaison des deux approches.

---

## Table des matières

1. [Contexte](#contexte)
2. [Objectifs](#objectifs)
3. [Équipe et rôles](#équipe-et-rôles)
4. [Données](#données)
5. [Structure du projet](#structure-du-projet)
6. [Installation et dépendances](#installation-et-dépendances)
7. [Utilisation](#utilisation)
8. [Résultats et limites](#résultats-et-limites)

---

## Contexte

L’analyse des profils atypiques sur les réseaux sociaux constitue un enjeu important pour la compréhension des comportements en ligne. Certains comptes peuvent présenter des caractéristiques particulières, par exemple une activité très élevée, une faible visibilité malgré de nombreuses publications, ou une utilisation importante de hashtags, de mentions et de liens externes. Ces comportements peuvent parfois être associés à des bots, des spammeurs ou des comptes diffusant massivement de l’information, mais les données disponibles ne permettent pas de les identifier avec certitude.

Ce projet s’inscrit dans la continuité de travaux portant sur la détection de profils suspects sur Twitter, notamment l’outil SPOT développé à l’UTT, qui propose d’analyser les profils selon plusieurs dimensions comme l’activité, la visibilité ou le niveau de risque. Dans notre cas, nous nous inspirons de cette logique en construisant des indicateurs statistiques et comportementaux à partir des variables disponibles dans le jeu de données.

Nous proposons d’expérimenter et de comparer deux familles d’approches sur un corpus de tweets collectés autour de l’événement Coupe du Monde :

une approche non supervisée, fondée sur le clustering du comportement des utilisateurs ;
une approche supervisée, fondée sur une classification à partir d’un étiquetage heuristique des profils.

L’enjeu n’est pas seulement de produire un modèle de classification, mais surtout de comparer la pertinence des deux approches pour identifier des profils qui s’écartent statistiquement de la population moyenne.

## Objectifs

- Construire un dataset analytique au **niveau profil utilisateur** à partir d’un dataset brut au niveau tweet.
- Comprendre les variables disponibles dans le jeu de données `Tweet_Worldcup` et sélectionner les indicateurs pertinents.
- Concevoir des **features descriptives** liées aux caractéristiques sociales, au comportement de publication et au contenu des tweets.
- Créer des indicateurs métier tels que le ratio followers/friends, le ratio d’activité et le score de visibilité.
- Implémenter et analyser une approche **non supervisée** basée sur K-Means afin d’identifier des groupes de profils aux comportements similaires.
- Implémenter et évaluer une approche **supervisée** basée sur Random Forest afin de détecter automatiquement des profils potentiellement atypiques.
- Comparer les deux approches selon plusieurs dimensions : interprétabilité, performance, robustesse, limites et coût de mise en œuvre.
- Rédiger un rapport de projet et préparer une soutenance orale.


## Équipe et rôles

| Membre | Rôles assumés | Contributions principales |
|---|---|---|
| HU Xinyu | Data Engineer | Prétraitement des données, extraction et analyse des variables, agrégation au niveau utilisateur, PCA et visualisation des profils |
| TIMADJI Jules | Data Analyst | Analyse des données, interprétation des résultats, rédaction et structuration du rapport |
| KAMGUE Ange | Machine Learning Engineer | Mise en place de l’approche non supervisée, application de K-Means, choix du nombre de clusters et interprétation des groupes |
| NOUGWA KEMAJOU Tatiana | Machine Learning Engineer | Mise en place de l’approche supervisée, construction des pseudo-labels, entraînement et évaluation du Random Forest |
| FOTIO Francky | Project Analyst | Comparaison entre K-Means et Random Forest, analyse critique des deux approches, limites et perspectives |



## Données

### Source

Jeu de données fourni dans le cadre de l'UE : `Tweet_Worldcup.zip` (≈ 27 Go décompressé), constitué de tweets collectés via l'API publique de Twitter pendant la Coupe du Monde, au format JSON.

## Structure du projet

Le dépôt est organisé en plusieurs modules correspondant aux différentes étapes du pipeline de traitement et d’analyse des données.

```text
IF29-Twitter-Classification/
│
├── Traitement_des_donnees/
│   ├── 1_create.ipynb
│   ├── 2_feature_extraction.ipynb
│   ├── 3_features_calc.ipynb
│   └── pca_explorer.ipynb
│
├── dataset/
│   ├── user_twitter_data.csv
│   ├── data_with_features.csv
│   └── data_with_pca.csv
│
├── Random forest/
│   └── Algorithme supervisé.ipynb
│
├── K-Means/
│   └── 4_non_supervise_KMeans.ipynb
│
├── Experimentation_Analyse_Comparative/
│   └── Experimentation_Analyse_Comparative.ipynb
│
├── README.md
├── .gitignore
└── .gitattributes
```


## Installation et dépendances

Le projet a été réalisé avec Python et Jupyter Notebook.

Les principales bibliothèques utilisées sont :

```text
pandas
numpy
scikit-learn
matplotlib
seaborn
pymongo
jupyter
```

Installation possible :

```bash
pip install pandas numpy scikit-learn matplotlib seaborn pymongo jupyter
```

## Utilisation

Les notebooks doivent être exécutés dans l’ordre suivant :

```text
1. Traitement_des_donnees/1_create.ipynb
2. Traitement_des_donnees/2_feature_extraction.ipynb
3. Traitement_des_donnees/3_features_calc.ipynb
4. Traitement_des_donnees/pca_explorer.ipynb
5. K-Means/4_non_supervise_KMeans.ipynb
6. Random forest/Algorithme supervisé.ipynb
7. Experimentation_Analyse_Comparative/Experimentation_Analyse_Comparative.ipynb
```

Les données nécessaires doivent être placées dans le dossier `dataset/`.

## Résultats et limites

Les deux approches apportent des informations complémentaires.

L’approche non supervisée avec K-Means permet d’explorer la structure des données et d’identifier différents groupes d’utilisateurs selon leurs comportements.

L’approche supervisée avec Random Forest permet de détecter automatiquement des profils potentiellement atypiques selon les critères définis dans le projet.

Cependant, les résultats doivent être interprétés avec prudence. Les labels utilisés pour l’approche supervisée sont construits à partir de règles métier et ne correspondent pas à des labels réels de bots ou de spammeurs. Le projet permet donc d’identifier des profils potentiellement atypiques, mais pas de certifier qu’un compte est malveillant.
