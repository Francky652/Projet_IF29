# IF29 — Classification de profils X (Twitter) atypiques

> Projet académique — **UE IF29 : Traitement de données (Data Analytics)**
> Université de Technologie de Troyes (UTT)
> Année universitaire 2025-2026

Comparaison de deux approches de Machine Learning — **supervisée** et **non supervisée** — pour la détection de profils suspects, automatisés ou atypiques sur la plateforme X (anciennement Twitter), à partir du jeu de données `Tweet_Worldcup`.

---
### Introduction du projet

Ce projet présente le travail effectué par le groupe 2 pour le projet de l’UE IF29 - Traitement de données (Data Analytics). L’objectif de ce projet est d’implémenter sur un même dataset et pour une même problématique une approche non supervisée et une approche supervisée et d’en faire le comparatif de Machine learning.

L'objectif est de mettre en œuvre une méthode non supervisée et une méthode supervisée pour le même problème sur le même ensemble de données et de les comparer.

Nous mettrons en œuvre deux algorithmes d'apprentissage automatique pour détecter les profils Twitter "suspects". Une étude comparative sera ensuite menée sur les résultats fournis par chacun des deux algorithmes sur les dimensions pertinentes. Ensuite, nous effectuons une optimisation améliorée.

Ce repository contient l'ensemble des ressources qui ont été utiles à la réalisation du projet d'IF29 nécessitant l'implémentation de deux méthodes de machine learning (supervisé et non supervisé) afin de trouver des profils twitter dit suspect.

---

## Table des matières

1. [Contexte](#contexte)
2. [Objectifs](#objectifs)
3. [Équipe et rôles](#équipe-et-rôles)
4. [Données](#données)
5. [Structure du projet](#structure-du-projet)

---

## Contexte

La détection de profils malveillants (bots, spammeurs, comptes diffuseurs de fake news) constitue un enjeu majeur de modération des réseaux sociaux. Ce projet s'inscrit dans la lignée des travaux fondateurs du domaine, notamment l'outil **SPOT** (Perez et al., 2011) développé à l'UTT, qui propose un scoring tridimensionnel des profils suspects selon trois axes : *agressivité*, *visibilité* et *dangerosité*.

Nous proposons d'expérimenter et de comparer deux familles d'approches sur un corpus de tweets collectés autour de l'événement *Coupe du Monde* :

- une **classification supervisée**, fondée sur un étiquetage heuristique des profils ;
- une **classification non supervisée**, fondée sur un clustering du comportement des utilisateurs.

L'enjeu n'est pas uniquement de produire un classifieur, mais surtout de **comparer la pertinence des deux approches** pour identifier des profils s'écartant statistiquement de la population moyenne.

## Objectifs

- Construire un dataset analytique au **niveau profil** à partir d'un dataset brut au niveau tweet (≈ 27 Go).
- Concevoir un ensemble de **features** descriptives (sociales, comportementales, contenu) inspirées de la littérature scientifique.
- Implémenter et évaluer **au moins un modèle supervisé** (Random Forest, SVM, Régression logistique).
- Implémenter et évaluer **au moins un modèle non supervisé** (K-Means, DBSCAN, clustering hiérarchique).
- Mener une **analyse comparative critique** des deux approches : performance, interprétabilité, robustesse, coût computationnel.
- Rédiger un **rapport scientifique** et préparer une **soutenance orale**.

## Équipe et rôles

| Membre | Rôles assumés | Contributions principales |
|---|---|---|
| *TIMADJI Jules* | Data Engineer | Import MongoDB, pipelines d'agrégation, nettoyage |
| *Fotio Francky* | Data Cleaner | Exploration, feature engineering, visualisations |
| *HU Xinyu* | Data Analyst, Dataviz Expert | Modèles supervisés, pipelines scikit-learn |
| *Nom 4* | ML Engineer | Modèles non supervisés, comparaison, rapport |
| *Nom 5* | ML Project Manager, Rédacteur | Modèles non supervisés, comparaison, rapport |



## Données

### Source

Jeu de données fourni dans le cadre de l'UE : `Tweet_Worldcup.zip` (≈ 27 Go décompressé), constitué de tweets collectés via l'API publique de Twitter pendant la Coupe du Monde, au format JSON.

## Structure du projet

Le dépôt est organisé en plusieurs modules correspondant aux différentes étapes du pipeline de traitement et d’analyse des données.Si le nom du fichier n’est pas approprié, merci de le modifier.

```text
IF29-Twitter-Classification/
│
├── Traitement_des_donnees/
│   ├── 1_create.ipynb
│   ├── 2_feature_extaction.ipynb
│   ├── 3_features_calc.ipynb
│   └── output.json
│
├── dataset/
│   ├── user_twitter_data.csv
│   ├── data_with_labels_train_std.csv
│   └── data_with_labels_predict_std.csv
│
├── SVM/
│   ├── EtiquettesMaker.ipynb
│   └── ML_SVM.ipynb
│
├── K-Means/
│   ├── Kmeans_sans_pca.ipynb
│   └── Kmeans_avec_pca.ipynb
│
├── pca_explorer.ipynb
├── README.md
├── .gitignore
└── .gitattributes
