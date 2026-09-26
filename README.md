<div align="center">
  <img src=".github/assets/banner.png" alt="NOVA_ML_PREPROD banner" width="100%" />

  <h1>NOVA_ML_PREPROD</h1>
  <p>Training and experimentation environment for the machine learning models of the NOVA health platform.</p>

<p>
  <img src="https://img.shields.io/github/last-commit/nova-health-platform/NOVA_ML_PREPROD" alt="last update" />
  <img src="https://img.shields.io/github/languages/top/nova-health-platform/NOVA_ML_PREPROD" alt="top language" />
</p>
</div>

<br />

## :notebook_with_decorative_cover: Table of Contents

- [About](#star2-about)
  * [Tech Stack](#space_invader-tech-stack)
  * [Features](#dart-features)
  * [Environment Variables](#key-environment-variables)
- [Getting Started](#toolbox-getting-started)
  * [Prerequisites](#bangbang-prerequisites)
  * [Run the Notebooks](#running-run-the-notebooks)
- [Related Repositories](#link-related-repositories)
- [Contact](#handshake-contact)

## :star2: About

NOVA_ML_PREPROD gathers the training notebooks used before shipping ML models to production in NOVA's dedicated repositories. This is where models are designed, tested and exported (joblib/Keras) before being picked up as-is by the production services.

The repository is organized into two folders, one per ML module:

- `ML_ANALYSIS`: training of the symptom analysis model. Data is pulled from PostgreSQL (the `dataset` table, fed by NOVA_DB), oversampled for underrepresented diseases, reduced in dimension (PCA) and passed through a Keras autoencoder before classification by a scikit-learn `MLPClassifier`. The trained artifacts (`model_analysis.joblib`, `scaler.joblib`, `pca.joblib`, `label_encoder.joblib`, `encoder.keras`, `symptom_columns.json`) are exported to `ML_ANALYSIS/joblibs` to be picked up by NOVA_ML_ANALYSIS. The `import_&_feed_tables.ipynb` notebook handles data import and preparation ahead of training.
- `ML_MENTAL_HEALTH`: psychological monitoring dataset (`data.csv`, a DASS-style questionnaire format with answers, intensity and response time per question). The training notebook for this module is still empty, the dataset is in place but the model has yet to be built.

### :space_invader: Tech Stack

<details>
  <summary>Machine Learning</summary>
  <ul>
    <li><a href="https://www.tensorflow.org/">TensorFlow / Keras</a> (autoencoder)</li>
    <li><a href="https://scikit-learn.org/">scikit-learn</a> (MLPClassifier, PCA, StandardScaler, LabelEncoder)</li>
    <li><a href="https://joblib.readthedocs.io/">Joblib</a> (model serialization)</li>
  </ul>
</details>

<details>
  <summary>Data</summary>
  <ul>
    <li><a href="https://pandas.pydata.org/">Pandas</a></li>
    <li><a href="https://numpy.org/">NumPy</a></li>
    <li><a href="https://www.postgresql.org/">PostgreSQL</a> (source of the symptom analysis data)</li>
  </ul>
</details>

<details>
  <summary>Visualization</summary>
  <ul>
    <li><a href="https://matplotlib.org/">Matplotlib</a></li>
    <li><a href="https://seaborn.pydata.org/">Seaborn</a></li>
  </ul>
</details>

<details>
  <summary>Environment</summary>
  <ul>
    <li><a href="https://jupyter.org/">Jupyter Notebook</a></li>
  </ul>
</details>

### :dart: Features

- Loading symptom/disease data from PostgreSQL and oversampling minority classes
- Dimensionality reduction via PCA followed by compression through a Keras autoencoder
- Hyperparameter search (`RandomizedSearchCV`) and training of an `MLPClassifier` to predict diseases from symptoms
- Model evaluation (F1-score, accuracy, confusion matrix, classification report)
- Export of models and preprocessing objects for reuse in production
- Demo prediction from a list of active symptoms, with the top 5 most likely diseases
- Dataset ready for a future psychological monitoring model (mental health module)

### :key: Environment Variables

The notebooks connect to PostgreSQL via a `.env` file (not versioned):

`DB_HOST`

`DB_NAME`

`DB_USER`

`DB_PASSWORD`

`DB_PORT`

## :toolbox: Getting Started

### :bangbang: Prerequisites

Python with Jupyter, and access to the PostgreSQL database fed by [NOVA_DB](https://github.com/nova-health-platform/NOVA_DB).

### :running: Run the Notebooks

Dependencies are installed directly in the first cell of the `model.ipynb` notebook (tensorflow, scikit-learn, joblib, matplotlib, pandas, numpy, seaborn, psycopg2-binary, python-dotenv, flask). Simply open the notebook and run the cells in order:

```bash
jupyter notebook ML_ANALYSIS/model.ipynb
```

## :link: Related Repositories

This repository is the training environment for the NOVA ecosystem, composed of several services:

- [NOVA_WEB](https://github.com/nova-health-platform/NOVA_WEB): web frontend of the platform
- [NOVA_API](https://github.com/nova-health-platform/NOVA_API): main NOVA API
- [NOVA_DB](https://github.com/nova-health-platform/NOVA_DB): reference database (symptoms, diseases, treatments)
- [NOVA_LOGS_DB](https://github.com/nova-health-platform/NOVA_LOGS_DB): application log storage
- [NOVA_ML_ANALYSIS](https://github.com/nova-health-platform/NOVA_ML_ANALYSIS): production service that embeds the model trained here
- [NOVA_ML_MENTAL_HEALTH](https://github.com/nova-health-platform/NOVA_ML_MENTAL_HEALTH): psychological monitoring module
- [NOVA_ML_SCAN_BODY](https://github.com/nova-health-platform/NOVA_ML_SCAN_BODY): computer vision dermatological check-up module
- [NOVA-CORE](https://github.com/nova-health-platform/NOVA-CORE): architecture overview and local orchestration for the whole platform

## :handshake: Contact

Brieuc Dumortier - [LinkedIn](https://www.linkedin.com/in/dumortier-brieuc/) - dumortier.contact@gmail.com

[https://github.com/BaditSad](https://github.com/BaditSad)
