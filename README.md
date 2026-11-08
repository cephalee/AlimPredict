# SAE Sciences des Données : prédire la synthèse d'une inspection sanitaire

## Le projet

On veut savoir si on peut prédire qu'une inspection sanitaire (données Alim'confiance) sera défavorable, à partir de l'établissement et de sa commune.

- Cible : `defavorable` = 1 si la synthèse est "A améliorer% ou "A corriger de manière urgente% (6,4% des inspections).
- Modèle A : variables de l'établissement seulement.
- Modèle B : modèle A + revenu médian, taux de pauvreté et population de la commune.
- Métriques : rappel, F1 et PR-AUC. Pas d'exactitude, car un modèle qui répond toujours "favorable% a 93,6%.

Où on en est : la préparation des données (S2) est faite. Il reste les graphiques (S3), les modèles (S4) et le rapport final avec la soutenance (S5).

## Données

- [Alim'confiance](https://www.data.gouv.fr/datasets/5593aab9c751df35d8a453ba/)
- [Filosofi 2023](https://www.insee.fr/fr/statistiques/8984752)
- [Populations 2023](https://www.insee.fr/fr/statistiques/8680726)

Les fichiers sources ne sont pas dans le dossier du projet, il faut les télécharger et les placer comme ci-dessous.

## Organisation du dossier

```
├── README.md
├── dictionnaire_variables.md
├── preprocessing.ipynb
├── export_alimconfiance.csv
├── FILOSOFI_CC_csv/DS_FILOSOFI_CC_2023_data.csv
├── ensemble/donnees_communes.csv
└── data/
    ├── train.csv
    └── test.csv
```

## Comment l'exécuter

1. Placer les fichiers sources comme dans le schéma.
2. Ouvrir `preprocessing.ipynb` depuis le dossier `sae/` (les chemins sont relatifs).
3. Exécuter toutes les cellules dans l'ordre. Le notebook modifie le même tableau `al` d'une étape à l'autre, donc il ne faut pas les lancer dans le désordre.

À la fin, `data/train.csv` et `data/test.csv` sont créés.

Pour les relire, mettre `SIRET` et `com_code` en texte, sinon les codes communes perdent leur zéro du début (`01001` devient `1001`) :

```python
train = pd.read_csv("data/train.csv", dtype={"SIRET": str, "com_code": str})
test = pd.read_csv("data/test.csv", dtype={"SIRET": str, "com_code": str})
```

## Dépendances

Python 3 avec pandas, numpy, matplotlib, scikit-learn et jupyter :

```
pip install pandas numpy matplotlib scikit-learn jupyter
```