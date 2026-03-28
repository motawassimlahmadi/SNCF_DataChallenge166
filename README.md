# Data Challenge 166 - Transilien SNCF Voyageurs

## Contexte

Transilien SNCF Voyageurs est l'operateur des trains de banlieue en Ile-de-France, faisant circuler plus de 6 200 trains par jour pour 3,4 millions de voyageurs. Pour ameliorer l'experience voyageur, un temps d'attente estime est affiche en gare. Ce challenge explore la possibilite d'ameliorer la qualite de ces previsions.

## Objectif

Predire a court terme la **difference entre le temps d'attente theorique et le temps d'attente realise** (`p0q0`) pour un train situe deux gares en amont.

- Si `p0q0 < 0` : le temps d'attente est plus long que prevu
- Si `p0q0 > 0` : le temps d'attente est plus court que prevu

## Donnees

### Structure

| Fichier | Lignes | Description |
|---------|--------|-------------|
| `x_train.csv` | 667 265 | Variables explicatives (entrainement) |
| `y_train.csv` | 667 265 | Variable cible (entrainement) |
| `x_test.csv` | 20 658 | Variables explicatives (test) |
| `y_sample.csv` | 20 658 | Exemple de soumission (valeurs aleatoires entre [-10, 10]) |

### Variables contextuelles

| Variable | Description |
|----------|-------------|
| `train` (k) | Numero de train (unique par jour) |
| `gare` (s) | Identifiant de la gare |
| `date` (d) | Date au format YYYY-MM-DD |
| `arret` | Numero de l'arret |

### Variables passees

| Variable | Description |
|----------|-------------|
| `p2q0` | Difference de temps d'attente du 2e train precedent (k-2) a la gare s |
| `p3q0` | Difference de temps d'attente du 3e train precedent (k-3) a la gare s |
| `p4q0` | Difference de temps d'attente du 4e train precedent (k-4) a la gare s |
| `p0q2` | Difference de temps d'attente du meme train k a la 2e gare precedente (s-2) |
| `p0q3` | Difference de temps d'attente du meme train k a la 3e gare precedente (s-3) |
| `p0q4` | Difference de temps d'attente du meme train k a la 4e gare precedente (s-4) |

> **Notation** : `p` definit les valeurs passees aux arrets precedents pour le meme train, `q` definit les valeurs passees pour le precedent train a la meme gare.

## Evaluation

- **Metrique** : MAE (Mean Absolute Error)
- **Benchmark** : Foret aleatoire classique

## Structure du projet

```
SNCF/
├── data/                  # Donnees d'entrainement et de test
├── notebooks/
│   ├── experiment.ipynb   # Exploration et visualisation des donnees
│   └── modelling.ipynb    # Entrainement des modeles (XGBoost, LightGBM, CatBoost)
├── output/                # Predictions generees
├── requirements.txt       # Dependances Python
└── README.md
```

## Approche

1. **EDA** : Analyse exploratoire et visualisation des donnees (`notebooks/experiment.ipynb`)
2. **Feature engineering** : Creation de variables supplementaires a partir des donnees contextuelles et passees
3. **Modelisation** : Ensemble de modeles (XGBoost, LightGBM, CatBoost) avec optimisation des hyperparametres via Optuna (`notebooks/modelling.ipynb`)
4. **Validation** : Cross-validation temporelle (TimeSeriesSplit)

## Resultats

Meilleur score obtenu : **MAE = 0.6559** - Top 10 %


