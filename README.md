# Municipal Solid Waste Forecasting

Miniprojet individuel réalisé dans le cadre du cours **3MD3090 – IA et Environnement** à CentraleSupélec (Avril 2026), enseigné par Gilles Faÿ.

---

## Objectif

Ce projet reproduit partiellement les résultats de :

> Mudannayake et al. (2022). *Exploring Machine Learning and Deep Learning Approaches for Multi-Step Forecasting in Municipal Solid Waste Generation*. IEEE Access, vol. 10, pp. 122570–122585.

Il étend ensuite le travail original avec trois contributions :
- Test d'un **foundation model de forecasting** (Chronos, Amazon 2024) en zero-shot et rolling forecast
- Reproduction de l'**approche multi-model** du papier (un modèle par jour de la semaine)
- **Mesure de l'empreinte carbone** de chaque modèle avec CodeCarbon, angle absent du papier original

---

## Résultats principaux

| Modèle | MAPE (%) | CO2 (g) |
|---|---|---|
| Linear Regression | 8.60 | 0.0003 |
| Multi-model LR | 8.17 | 0.0004 |
| Prophet | 11.03 | 0.0007 |
| Random Forest | 11.73 | 0.0363 |
| Chronos zero-shot | 23.32 | 0.0577 |
| Chronos rolling h=7 | **6.87** | 0.1298 |
| Chronos multi-model h=7 | 7.11 | 0.9409 |

> MAPE calculée sur les jours de semaine uniquement (lundi–vendredi), les weekends étant sans collecte à Austin.

**Conclusion** : sur ce dataset, les modèles classiques bien préprocessés suffisent. Chronos rolling h=7 offre la meilleure MAPE absolue mais à un coût carbone 430x supérieur à Linear Regression. La sophistication computationnelle ne se justifie pas sans tenir compte de l'empreinte environnementale.

---

## Dataset

Dataset public : **Waste Collection & Diversion Report (daily)** — Ville d'Austin, Texas  
Source : https://data.austintexas.gov/Utilities-and-City-Services/Waste-Collection-Diversion-Report-daily-/mbnu-4wq9

Le dataset (~87 Mo) n'est pas inclus dans ce repo. Il est **téléchargé automatiquement** via l'API Socrata au premier lancement du notebook et sauvegardé localement sous `austin_waste.csv`.

---

## Structure du repo

```
msw-forecasting/
├── msw_forecasting.ipynb   # notebook principal
├── requirements.txt        # dépendances
├── README.md
└── .gitignore
```

---

## Installation

```bash
git clone https://github.com/comegenet/msw-forecasting.git
cd msw-forecasting
pip install -r requirements.txt
pip install git+https://github.com/amazon-science/chronos-forecasting.git
jupyter notebook msw_forecasting.ipynb
```

---

## Structure du notebook

| Section | Contenu |
|---|---|
| 0 | Imports |
| 1 | Chargement des données |
| 2 | Exploration et visualisation |
| 3 | Preprocessing (outliers par jour, imputation, split 70/30) |
| 4 | Modèles : Linear Regression, Random Forest, Prophet |
| 5 | Comparaison avec le papier |
| 6 | Extension — Chronos zero-shot + rolling forecast |
| 7 | Extension — Approche multi-model |
| 8 | Extension — Chronos multi-model |
| 9 | Extension — Empreinte carbone (CodeCarbon) |

---

## Dépendances principales

```
darts==0.43.0
prophet==1.3.0
codecarbon==3.2.5
chronos-forecasting==2.2.2
plotly==6.6.0
pytorch-lightning==2.6.1
```

Voir `requirements.txt` pour la liste complète.

---

## Référence

Mudannayake, O., Rathnayake, D., Herath, J. D., Fernando, D. K., & Fernando, M. (2022).
Exploring Machine Learning and Deep Learning Approaches for Multi-Step Forecasting in Municipal Solid Waste Generation.
*IEEE Access*, 10, 122570–122585. https://doi.org/10.1109/ACCESS.2022.3221941
