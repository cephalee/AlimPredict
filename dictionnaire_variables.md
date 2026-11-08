# Dictionnaire des variables

Colonnes de `data/train.csv` et `data/test.csv` (71 151 inspections au total, 18 colonnes). Les deux fichiers ont les mêmes colonnes. Les variables de la partie 2 sont utilisées par le modèle A et par le modèle B, celles de la partie 3 seulement par le modèle B.

## 1. Cible

| Nom | Type | Description | Valeurs | Source |
| --- | --- | --- | --- | --- |
| `defavorable` | entier 0/1 | Cible : 1 si la synthèse est « A améliorer » ou « A corriger de manière urgente » | 1 pour 4 523 inspections (6,4 %) | Créée à partir de `Synthese_eval_sanit` (Alim'confiance) |

## 2. Variables de l'établissement et de l'inspection (modèles A et B)

| Nom | Type | Description | Valeurs | Source |
| --- | --- | --- | --- | --- |
| `Contexte_inspection` | texte | Raison de l'inspection, connue avant le contrôle | 6 modalités : Programmation, Re-contrôle, Inspection ciblée, Plainte / Signalement, Demande d'approbation, TIAC | Alim'confiance |
| `Secteur_activite` | texte | Secteur réglementaire de l'établissement | 5 modalités : Remise directe, Mise sur le marché, Restauration collective, Abattage, Élevage ou détention animaux de rente | Alim'confiance |
| `ods_type_activite` | texte | Catégorie d'activité | 7 modalités : Autres, Viandes et produits carnés, Lait et produits laitiers, Produits de la mer et d'eau douce, Transport et entreposage de denrées alimentaires, Œufs et ovoproduits, Abattoirs | Alim'confiance |
| `APP_Libelle_regroupement_etablissement` | texte | Boutique ou grande surface | Boutique, Supermarché/Hypermarché | Alim'confiance |
| `activite` | texte | Type d'activité détaillé | 19 modalités + « autre » (types avec moins de 100 inspections). Les plus fréquentes : Restauration commerciale (33 068), Métier de bouche (14 629) | Créée à partir de `Type_activite` |
| `enseigne` | entier | Type d'enseigne | 0 : sans enseigne (3 900) ; 1 : enseigne rare (63 752) ; 2 : parmi les 20 plus fréquentes (3 499) | Créée à partir de `#Enseigne` |
| `Agrement` | entier 0/1 | 1 si un agrément est renseigné | environ 11 % de 1 | Créée à partir de `Agrement` |
| `annee` | entier | Année de l'inspection | 2025 (12 978) ou 2026 (58 173), presque redondante avec `mois` | Extraite de `Date_inspection` |
| `mois` | entier | Mois de l'inspection | 1 à 12 (octobre 2025 et octobre 2026 sont partiels) | Extraite de `Date_inspection` |
| `jour_semaine` | entier | Jour de la semaine | 0 (lundi) à 6 (dimanche) | Extraite de `Date_inspection` |

## 3. Indicateurs de la commune (modèle B)

Ils décrivent la commune, pas l'établissement : tous les établissements d'une même commune ont les mêmes valeurs. Les pourcentages de valeurs manquantes sont calculés sur les lignes qui ont un `com_code`.

| Nom | Type | Description | Valeurs manquantes | Source |
| --- | --- | --- | --- | --- |
| `revenu_median` | décimal | Revenu médian de la commune, en euros (`MED_SL`) | 1,5 %, surtout l'outre-mer (971, 972, 973, 976) | Filosofi 2023 |
| `taux_pauvrete` | décimal | Taux de pauvreté de la commune, en % (`PR_MD60`) | 19,9 %, petites communes sous secret statistique | Filosofi 2023 |
| `population` | décimal | Population municipale (`PMUN`). Paris, Lyon et Marseille sont obtenues en additionnant leurs arrondissements | 0,2 % | Populations 2023 |

## 4. Identifiants et colonnes à ne pas utiliser comme variables

| Nom | Type | Description | Remarque | Source |
| --- | --- | --- | --- | --- |
| `SIRET` | texte | Identifiant de l'établissement (64 575 valeurs différentes dans le jeu brut) | Sert à séparer train et test. À lire en texte | Alim'confiance |
| `com_code` | texte | Code commune INSEE (5 caractères) | Sert à la jointure. À lire en texte. Absent pour 2,5 % des lignes | Alim'confiance |
| `Date_inspection` | date | Date de l'inspection, du 25 octobre 2025 au 23 octobre 2026 | Sert à créer `annee`, `mois` et `jour_semaine`. À reconvertir avec `pd.to_datetime(..., utc=True)` après lecture du CSV | Alim'confiance |
| `code_synthese` | entier 1 à 4 | Synthèse codée : 1 Très satisfaisant (17 119), 2 Satisfaisant (49 509), 3 A améliorer (4 184), 4 A corriger de manière urgente (339) | Fuite de cible : seulement pour les graphiques, jamais comme variable du modèle | Créée à partir de `Synthese_eval_sanit` |
