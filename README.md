<div align="center">
  <h1>NOVA ML Preprod</h1>
  <p>Environnement d'entrainement et d'experimentation des modeles de machine learning de la plateforme de sante NOVA.</p>

<p>
  <img src="https://img.shields.io/github/last-commit/BaditSad/NOVA_ML_PREPROD" alt="last update" />
  <img src="https://img.shields.io/github/languages/top/BaditSad/NOVA_ML_PREPROD" alt="top language" />
</p>
</div>

<br />

# Table des matieres

- [A propos](#a-propos)
  * [Stack technique](#stack-technique)
  * [Fonctionnalites](#fonctionnalites)
  * [Variables d'environnement](#variables-denvironnement)
- [Demarrage](#demarrage)
  * [Prerequis](#prerequis)
  * [Lancer les notebooks](#lancer-les-notebooks)
- [Depots lies](#depots-lies)
- [Contact](#contact)

## A propos

NOVA ML Preprod regroupe les notebooks d'entrainement utilises avant de livrer les modeles ML en production sur les depots dedies de NOVA. C'est ici que les modeles sont concus, testes et exportes (joblib/Keras) avant d'etre repris tels quels par les services de production.

Le depot est organise en deux dossiers, un par module ML :

- `ML_ANALYSIS` : entrainement du modele d'analyse de symptomes. Les donnees sont recuperees depuis PostgreSQL (table `dataset`, alimentee par NOVA DB), sur-echantillonnees pour les maladies sous representees, reduites en dimension (PCA) puis passees dans un autoencodeur Keras avant classification par un `MLPClassifier` scikit-learn. Les artefacts entraines (`model_analysis.joblib`, `scaler.joblib`, `pca.joblib`, `label_encoder.joblib`, `encoder.keras`, `symptom_columns.json`) sont exportes dans `ML_ANALYSIS/joblibs` pour etre repris par NOVA_ML_ANALYSIS. Le notebook `import_&_feed_tables.ipynb` gere l'import/la preparation des donnees en amont de l'entrainement.
- `ML_MENTAL_HEALTH` : jeu de donnees de suivi psychologique (`data.csv`, format questionnaire type DASS avec reponses, intensite et temps de reponse par question). Le notebook d'entrainement de ce module est encore vide, le dataset est en place mais le modele reste a construire.

### Stack technique

<details>
  <summary>Machine Learning</summary>
  <ul>
    <li><a href="https://www.tensorflow.org/">TensorFlow / Keras</a> (autoencodeur)</li>
    <li><a href="https://scikit-learn.org/">scikit-learn</a> (MLPClassifier, PCA, StandardScaler, LabelEncoder)</li>
    <li><a href="https://joblib.readthedocs.io/">Joblib</a> (serialisation des modeles)</li>
  </ul>
</details>

<details>
  <summary>Donnees</summary>
  <ul>
    <li><a href="https://pandas.pydata.org/">Pandas</a></li>
    <li><a href="https://numpy.org/">NumPy</a></li>
    <li><a href="https://www.postgresql.org/">PostgreSQL</a> (source des donnees d'analyse de symptomes)</li>
  </ul>
</details>

<details>
  <summary>Visualisation</summary>
  <ul>
    <li><a href="https://matplotlib.org/">Matplotlib</a></li>
    <li><a href="https://seaborn.pydata.org/">Seaborn</a></li>
  </ul>
</details>

<details>
  <summary>Environnement</summary>
  <ul>
    <li><a href="https://jupyter.org/">Jupyter Notebook</a></li>
  </ul>
</details>

### Fonctionnalites

- Chargement des donnees de symptomes/maladies depuis PostgreSQL et augmentation des classes minoritaires
- Reduction de dimension par PCA puis compression par autoencodeur Keras
- Recherche d'hyperparametres (`RandomizedSearchCV`) et entrainement d'un `MLPClassifier` pour la prediction de maladies a partir de symptomes
- Evaluation du modele (F1-score, accuracy, matrice de confusion, rapport de classification)
- Export des modeles et objets de pretraitement pour reutilisation en production
- Prediction de demonstration a partir d'une liste de symptomes actives, avec top 5 des maladies les plus probables
- Jeu de donnees pret pour un futur modele de suivi psychologique (module mental health)

### Variables d'environnement

Les notebooks se connectent a PostgreSQL via un fichier `.env` (non versionne) :

`DB_HOST`

`DB_NAME`

`DB_USER`

`DB_PASSWORD`

`DB_PORT`

## Demarrage

### Prerequis

Python avec Jupyter, un acces a la base PostgreSQL alimentee par [NOVA_DB](https://github.com/BaditSad/NOVA_DB).

### Lancer les notebooks

Les dependances sont installees directement en premiere cellule du notebook `model.ipynb` (tensorflow, scikit-learn, joblib, matplotlib, pandas, numpy, seaborn, psycopg2-binary, python-dotenv, flask). Il suffit d'ouvrir le notebook et d'executer les cellules dans l'ordre :

```bash
jupyter notebook ML_ANALYSIS/model.ipynb
```

## Depots lies

Ce depot est l'environnement d'entrainement de l'ecosysteme NOVA, compose de plusieurs services :

- [NOVA_WEB](https://github.com/BaditSad/NOVA_WEB) : frontend web de la plateforme
- [NOVA_API](https://github.com/BaditSad/NOVA_API) : API principale de NOVA
- [NOVA_DB](https://github.com/BaditSad/NOVA_DB) : base de donnees de reference (symptomes, maladies, traitements)
- [NOVA_LOGS_DB](https://github.com/BaditSad/NOVA_LOGS_DB) : stockage des logs applicatifs
- [NOVA_ML_ANALYSIS](https://github.com/BaditSad/NOVA_ML_ANALYSIS) : service de production qui embarque le modele entraine ici
- [NOVA_ML_MENTAL_HEALTH](https://github.com/BaditSad/NOVA_ML_MENTAL_HEALTH) : module de suivi psychologique
- [NOVA_ML_SCAN_BODY](https://github.com/BaditSad/NOVA_ML_SCAN_BODY) : module de check-up dermatologique par computer vision

## Contact

Brieuc Dumortier - [LinkedIn](https://www.linkedin.com/in/dumortier-brieuc/) - dumortier.contact@gmail.com

[https://github.com/BaditSad](https://github.com/BaditSad)
