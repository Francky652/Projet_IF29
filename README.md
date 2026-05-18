# IF29 — Classification de profils X (Twitter) atypiques

> Projet académique — **UE IF29 : Traitement de données (Data Analytics)**
> Université de Technologie de Troyes (UTT)
> Année universitaire 2025-2026

Comparaison de deux approches de Machine Learning — **supervisée** et **non supervisée** — pour la détection de profils suspects, automatisés ou atypiques sur la plateforme X (anciennement Twitter), à partir du jeu de données `Tweet_Worldcup`.

---

## Table des matières

1. [Contexte](#contexte)
2. [Objectifs](#objectifs)
3. [Équipe et rôles](#équipe-et-rôles)
4. [Données](#données)
5. [Méthodologie](#méthodologie)
6. [Structure du dépôt](#structure-du-dépôt)
7. [Installation](#installation)
8. [Utilisation](#utilisation)
9. [Résultats](#résultats)
10. [Limites et perspectives](#limites-et-perspectives)
11. [Références](#références)
12. [Licence](#licence)

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
| *TIMADJI Jules* | Data Engineer, Data Cleaner | Import MongoDB, pipelines d'agrégation, nettoyage |
| *Fotio Francky* | Data Analyst, Dataviz Expert | Exploration, feature engineering, visualisations |
| *Nom 3* | ML Engineer | Modèles supervisés, pipelines scikit-learn |
| *Nom 4* | ML Project Manager, Rédacteur | Modèles non supervisés, comparaison, rapport |



## Données

### Source

Jeu de données fourni dans le cadre de l'UE : `Tweet_Worldcup.zip` (≈ 27 Go décompressé), constitué de tweets collectés via l'API publique de Twitter pendant la Coupe du Monde, au format JSON.

### Stockage

Compte tenu du volume et du caractère semi-structuré (JSON imbriqué) des données, nous avons retenu **MongoDB** comme système de stockage. L'agrégation au niveau profil est réalisée via le moteur natif de Mongo (`aggregation pipeline`), avant export en format Parquet pour la phase analytique.

```
Tweets bruts (≈ 27 Go, MongoDB)
        │
        │  agrégation par user.id
        ▼
Dataset analytique (1 ligne / profil, Parquet)
        │
        │  feature engineering
        ▼
Matrice de features (X) prête pour scikit-learn
```

### Features construites

Conformément à la littérature, les features sont regroupées en trois familles :

**Features sociales**
- `followers_count`, `friends_count`
- `followers_friends_ratio`
- `reputation = followers / (followers + friends)`

**Features comportementales**
- `account_age_days`
- `tweet_frequency` (tweets/jour)
- `retweet_rate`, `reply_rate`
- `activity_hours_entropy` (régularité horaire)

**Features de contenu**
- `avg_hashtags_per_tweet`
- `avg_mentions_per_tweet`
- `avg_urls_per_tweet`
- `avg_tweet_length`
- `inter_tweet_similarity` (distance moyenne entre tweets d'un même profil)

**Features inspirées de SPOT** (Perez et al., 2011)
- `aggressiveness` = (f_tweets + f_friends) / f_max
- `visibility` = pondération de l'usage stratégique des `@` et `#`
- `danger_proxy` = proportion de tweets contenant des URL (proxy faute d'analyseur d'URL)

## Méthodologie

### 1. Exploration et préparation

- Inspection de la structure JSON, identification des champs pertinents.
- Calcul du nombre d'utilisateurs uniques et de la distribution `tweets/utilisateur`.
- Filtrage des profils sous-représentés (seuil minimal de tweets pour garantir la fiabilité des agrégats).

### 2. Pseudo-labellisation (pour le supervisé)

En l'absence d'un *ground truth* fourni, une **classe "suspect"** est définie par règles heuristiques cumulatives inspirées de SPOT et de la littérature (ex. fréquence anormalement haute, ratio followers/followings très bas, forte similarité entre tweets). Cette labellisation est volontairement conservatrice, et ses limites sont discutées dans le rapport.

### 3. Modélisation supervisée

- Pipeline `sklearn` avec `StandardScaler` + classifieur.
- Modèles testés : Logistic Regression (baseline interprétable), Random Forest (non linéaire, robuste), SVM (RBF).
- Validation croisée stratifiée 5-folds.
- Métriques : Accuracy, Precision, Recall, F1-score, ROC-AUC, matrice de confusion.

### 4. Modélisation non supervisée

- Normalisation des features.
- **K-Means** avec choix de k via la méthode du coude et le score de silhouette.
- **DBSCAN** pour détection de profils en marge (potentiellement atypiques).
- Visualisation par projection 2D (PCA, t-SNE).

### 5. Comparaison

- Tableau de synthèse : performances, interprétabilité, temps de calcul.
- Matrice de correspondance entre labels supervisés et clusters non supervisés.
- Discussion qualitative : dans quels cas chaque approche est-elle préférable ?

## Structure du dépôt

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── raw/                    # données brutes (non versionnées)
│   ├── interim/                # exports MongoDB intermédiaires
│   └── processed/              # dataset analytique final
├── notebooks/
│   ├── 01_exploration.ipynb           # exploration MongoDB
│   ├── 02_aggregation.ipynb           # construction du dataset profils
│   ├── 03_feature_engineering.ipynb   # calcul des features
│   ├── 04_eda.ipynb                   # analyse exploratoire univariée/bivariée
│   ├── 05_supervised.ipynb            # modèles supervisés
│   ├── 06_unsupervised.ipynb          # modèles non supervisés
│   └── 07_comparison.ipynb            # synthèse comparative
├── src/
│   ├── __init__.py
│   ├── mongo_utils.py          # fonctions de connexion et agrégation
│   ├── features.py             # calcul des features
│   ├── labeling.py             # heuristiques de pseudo-labellisation
│   └── evaluation.py           # métriques et visualisations
├── reports/
│   ├── rapport.pdf
│   └── soutenance.pdf
└── figures/                    # graphiques exportés
```

## Installation

### Prérequis

- Python ≥ 3.10
- MongoDB ≥ 6.0 (instance locale ou distante)
- Au moins 8 Go de RAM (16 Go recommandés)
- Le dataset `Tweet_Worldcup.zip` importé dans une base MongoDB locale

### Mise en place

```bash
# Cloner le dépôt
git clone https://github.com/<votre-org>/IF29-twitter-classification.git
cd IF29-twitter-classification

# Créer l'environnement virtuel
python -m venv .venv
source .venv/bin/activate          # Linux / macOS
# .venv\Scripts\activate           # Windows

# Installer les dépendances
pip install -r requirements.txt

# Lancer Jupyter
jupyter lab
```

### Dépendances principales

```
pymongo>=4.6
pandas>=2.1
numpy>=1.26
scikit-learn>=1.4
matplotlib>=3.8
seaborn>=0.13
plotly>=5.18
pyarrow>=14.0
tqdm>=4.66
jupyterlab>=4.0
```

## Utilisation

Exécuter les notebooks dans l'ordre numérique :

1. **`01_exploration.ipynb`** — Connexion à MongoDB et inspection de la structure des documents.
2. **`02_aggregation.ipynb`** — Pipeline d'agrégation MongoDB → export `profiles.parquet`.
3. **`03_feature_engineering.ipynb`** — Calcul des features dérivées (SPOT, ratios, etc.).
4. **`04_eda.ipynb`** — Analyse exploratoire et visualisations.
5. **`05_supervised.ipynb`** — Entraînement et évaluation des modèles supervisés.
6. **`06_unsupervised.ipynb`** — Clustering et détection d'outliers.
7. **`07_comparison.ipynb`** — Comparaison croisée des deux approches.

### Configuration

La connexion MongoDB est paramétrable via le fichier `src/mongo_utils.py` :

```python
MONGO_URI = "mongodb://localhost:27017/"
DB_NAME = "worldcup"
COLLECTION_NAME = "tweets"
```

## Résultats

> *Section à compléter une fois les expérimentations terminées.*

### Synthèse des performances

| Modèle | Type | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|---|
| Logistic Regression | Supervisé | — | — | — | — | — |
| Random Forest | Supervisé | — | — | — | — | — |
| SVM (RBF) | Supervisé | — | — | — | — | — |
| K-Means (k=—) | Non supervisé | n/a | — | — | — | n/a |
| DBSCAN | Non supervisé | n/a | — | — | — | n/a |

### Visualisations clés

> Insertion des principales figures du rapport (projection PCA, silhouette, matrice de confusion, etc.)

## Limites et perspectives

- **Pseudo-labellisation** : l'absence de vérité terrain rend l'évaluation du supervisé partiellement circulaire.
- **Snapshot temporel** : les features sont agrégées sur une période courte (Coupe du Monde), ce qui peut biaiser l'estimation de l'ancienneté ou de la fréquence.
- **Dangerosité** : l'analyse d'URL malveillantes (3ᵉ axe SPOT) n'est pas réalisée faute de service tiers de classification d'URL.
- **Pistes** : intégrer une analyse de réseau (graphe de mentions/retweets), enrichir avec un modèle de langage pour détecter les contenus générés automatiquement.

## Références

1. Perez, C., Lemercier, M., Birregah, B., & Corpel, A. (2011). *SPOT 1.0: Scoring Suspicious Profiles On Twitter*. In **Proceedings of ASONAM 2011**, IEEE, pp. 377–381.
2. Ferrara, E., Varol, O., Davis, C., Menczer, F., & Flammini, A. (2016). *The rise of social bots*. **Communications of the ACM**, 59(7), 96–104.
3. Chu, Z., Gianvecchio, S., Wang, H., & Jajodia, S. (2012). *Detecting automation of Twitter accounts: Are you a human, bot, or cyborg?* **IEEE TDSC**, 9(6), 811–824.
4. Varol, O., Ferrara, E., Davis, C. A., Menczer, F., & Flammini, A. (2017). *Online human-bot interactions: Detection, estimation, and characterization*. In **Proceedings of ICWSM**, 280–289.
5. Benevenuto, F., Magno, G., Rodrigues, T., & Almeida, V. (2010). *Detecting spammers on Twitter*. In **Proc. CEAS**.

## Licence

Projet réalisé dans un cadre académique (UE IF29 — UTT). Code distribué sous licence **MIT**. Les données ne sont pas redistribuées.

---
