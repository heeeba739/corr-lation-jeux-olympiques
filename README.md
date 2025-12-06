# 🏅 Analyse de Corrélation — Jeux Olympiques & Performance Socio-Économique

## 🎯 Objectif du projet
Ce projet vise à analyser la relation entre la **performance sportive des pays aux Jeux Olympiques** (médailles) et leurs **indicateurs socio-économiques** (PIB, population, urbanisation, etc.) à l’aide des données de la **World Bank (WDI)**.

Le projet génère :
- Des données nettoyées
- Des KPI de performance
- Des fichiers exploitables pour la visualisation et l’analyse

---


---

## 📥 Données utilisées

### 1. Données sportives
- `athlete_events.csv` : Données complètes des Jeux Olympiques
- `noc_regions.csv` : Correspondance entre les codes NOC et les pays

### 2. Données socio-économiques (API World Bank)
Quelques indicateurs utilisés :
- Population totale, active, âgée
- PIB total, PIB/habitant, croissance
- Urbanisation, densité de population
- Population féminine active

---

## ⚙️ Prérequis techniques

- Python:pour le nettoyage 
- MySQL:pour calcul des KPI
- Tableuax: pour la visualisation

  ----
  ## KPI choisis
  # Analyse de corrélation Jeux Olympiques & Performance Socio‑Économique

## KPI actionnables:

| Indicateur | Répond à la question |
|------------|---------------------|
| Médailles totales | Combien de médailles ce pays a-t-il remportées ? |
| Médaille-score pondéré | Quelle est la qualité globale des médailles (or/argent/bronze) de ce pays ? |
| Médaille-score pondéré / million habitants (Score pondéré / (Population / 1 000 000)) | Est-ce que ce pays gagne plus ou moins de médailles que ce qu’on aurait prédit simplement avec sa population ? |
| Ratio médailles / urbanisation (Médailles ÷ % population urbaine) | Les pays plus urbanisés sont-ils plus performants aux Jeux ? |
| Ratio sportivité / densité (Médailles ÷ (Population ÷ Superficie)) | La densité de population influence-t-elle la performance sportive ? |
| Médailles / PIB | Est ce que un pays “utilise bien” sa richesse pour produire de la performance olympique (sans prendre en compte la qualité des médailles ?) |
| Score pondéré / PIB_en_milliards (Médailles ÷ (PIB / 1e9)) | Ce pays obtient-il des médailles de grande valeur malgré un PIB limité ? |
| Taux participation aux JO par habitant (Athlètes ÷ population 15–64 ans)| Quelle proportion de la population active participe réellement aux Jeux ? |
| Score efficacité composite (Score pondéré ÷ population × PIB per capita)| Ce pays est-il performant malgré sa taille et son niveau de richesse ? |
| Médailles / PIB par habitant| Les pays riches par habitant sont-ils plus efficaces sportivement ? |
| Ratio médailles / population active (Médailles ÷ population 15–64 ans)| La population active (en âge de travailler) contribue-t-elle à de meilleures performances ? |
| Médailles / croissance PIB annuelle| La dynamique économique (croissance) influence-t-elle la performance aux Jeux ? |
| Ratio participation / femmes (Athlètes femmes ÷ population femmes 15–64 ans)| Les femmes sont-elles proportionnellement bien représentées parmi les athlètes ? |
| Ratio médaille / densité urbaine (Médailles ÷ (Population urbaine ÷ Superficie urbaine))| Est-ce qu’un pays très urbanisé produit plus ou moins de médailles qu’on ne le penserait ? |



## Codes WDI nécessaires pour les KPI actionnables :

| Nom | Code WDI |
|------------|---------------------|
|Population totale|SP.POP.TOTL|
|Population 65–UP ans|SP.POP.65UP.TO|
|Population 15–64 ans|SP.POP.1564.TO|
|Population urbaine (%)|SP.URB.TOTL.IN.ZS|
|PIB total (US$)|NY.GDP.MKTP.CD|
|PIB par habitant|NY.GDP.PCAP.CD|
|Croissance annuelle du PIB (%)|NY.GDP.MKTP.KD.ZG|
|Taux de participation des femmes à la population active (%)|SL.TLF.CACT.FE.ZS|
|densite|EN.POP.DNST|
|densite urbaine|EN.URB.LCTY|

-------
# Importation et nettoyage de données

   ### Importation des données avec python 

| nom de fichier | type | bibliothéque utilisée | description |
|----------------|------|-----------------------|-------------|
| athlete_event | csv | pandas, numpy | athlètes, médailles, disciplines, années, etc|
| noc_regions | csv | pandas numpy | correspondance codes NOC / pays ou régions |
| World Development Indicators (WDI) | API | Request, time| PIB, population, taux d’alphabétisation, accès à l’électricité, etc. |

# Nettoyage des données 
   ### Informations sur les table de données 
  ### Fonction utuliser
  ## table athléte:
_ `info()`:   ----------------------------------------------------------------------------_ `describe()`:                                                                      |  
<img width="332" height="464" alt="image" src="https://github.com/user-attachments/assets/cb8b12bc-eca2-4d9b-85b2-9e75ad7c6bb1" />  |          <img width="542" height="378" alt="image" src="https://github.com/user-attachments/assets/358dfc1f-218d-4760-a6f7-e098fc6be907" />

  ## Table Region:
  _ `info()`:

<img width="374" height="350" alt="image" src="https://github.com/user-attachments/assets/c0a31571-ae3a-4ec7-abc7-e908594635a5" />

## Table des API

_`info()`:

<img width="461" height="422" alt="image" src="https://github.com/user-attachments/assets/a949c04f-4cc2-4f14-912d-5b397b42f362" />

### Nettoyage des données:

# Résumé des modifications et méthodes de nettoyage

| Étape (modification) | Objectif / Remarque | Extrait de code | Méthode(s) / fonction(s) utilisées |
|---|---:|---|---|
| Lecture des fichiers | Charger les CSV en DataFrame | `df = pd.read_csv('athlete_events.csv')` | `pandas.read_csv` |
| Renommer colonne pour homogénéité | Uniformiser le nom de colonne (`region` → `country`) pour le merge | `noc = noc.rename(columns={"region":"country"})` | `DataFrame.rename` |
| Merge NOC → pays | Ajouter colonne `country` à `df` depuis `noc_regions` | `df = df.merge(noc[["NOC","country"]], on="NOC", how="left")` | `DataFrame.merge` |
| Remplacer pays manquant | Eviter `NaN` dans `country` en attribuant `Unknown` | `df["country"] = df["country"].fillna("Unknown")` | `Series.fillna` |
| Remplacer `Medal` manquant | Indiquer explicitement qu'il n'y a pas de médaille (`None`) | `df["Medal"] = df["Medal"].fillna("None")` | `Series.fillna` |
| Remplacer NA numériques par médiane | Remplacer `NaN` dans `Age`, `Height`, `Weight` par la médiane de la colonne | ```python\nfor col in ["Age","Height","Weight"]:\n    df[col] = df[col].fillna(df[col].median())\n``` | `Series.median`, `Series.fillna` |
| Vérifier doublons (compte) | Connaître le nombre de lignes dupliquées avant nettoyage | `df.duplicated().sum()` | `DataFrame.duplicated` |
| Suppression des doublons | Éliminer les enregistrements exacts en double | `df = df.drop_duplicates()` | `DataFrame.drop_duplicates` |
| Vérification des valeurs manquantes | Contrôle final des `NaN` par colonne | `print(df.isna().sum())` | `DataFrame.isna`, `Series.sum` |


 # Comparaison des valeurs manquantes — Table athlete

| Colonne   | Valeurs manquantes AVANT | Valeurs manquantes APRÈS |
|-----------|--------------------------|---------------------------|
| ID        | 0                        | 0                         |
| Name      | 0                        | 0                         |
| Sex       | 0                        | 0                         |
| Age       | 9474                     | 0                         |
| Height    | 60171                    | 0                         |
| Weight    | 62875                    | 0                         |
| Team      | 0                        | 0                         |
| NOC       | 0                        | 0                         |
| Games     | 0                        | 0                         |
| Year      | 0                        | 0                         |
| Season    | 0                        | 0                         |
| City      | 0                        | 0                         |
| Sport     | 0                        | 0                         |
| Event     | 0                        | 0                         |
| Medal     | 231333                   | 0                         |
| country   | —                        | 0                         |

   ## Conclusion       
- Les colonnes Age, Height et Weight contenaient un volume important de valeurs manquantes.
- Ces valeurs ont été remplacées par la médiane de chaque colonne.
- La colonne Medal contenait plus de 231 000 valeurs manquantes, remplacées par la valeur "None".
- Aucune valeur manquante n’existe après le nettoyage.
- La colonne country a été ajoutée via une jointure avec la table NOC.




# Nettoyage et Extraction des Données via l’API World Bank (WDI)

| Étape | Élément | Méthode / Fonction | Description |
|------|---------|---------------------|-------------|
| 1 | Sélection des indicateurs | `indicators = {...}` | Liste validée des indicateurs démographiques, économiques et urbains à extraire depuis l’API WDI. |
| 2 | Appel API | `requests.get(url)` | Envoi de la requête HTTP vers l’API World Bank pour récupérer les données. |
| 3 | Gestion de la pagination | `while True` + `page` | Récupération automatique de toutes les pages de résultats de l’API. |
| 4 | Conversion JSON | `response.json()` | Transformation de la réponse de l’API en format exploitable (JSON). |
| 5 | Filtrage des valeurs manquantes | `if d["value"] is not None` | Suppression des valeurs nulles avant l’enregistrement dans le DataFrame. |
| 6 | Sélection des colonnes | Dictionnaire Python | Conservation uniquement des champs : `country_code`, `country`, `year`, et l’indicateur concerné. |
| 7 | Conversion du format de l’année | `int(d["date"])` | Transformation de l’année en format numérique. |
| 8 | Stockage temporaire | `records.append({...})` | Enregistrement ligne par ligne des données récupérées. |
| 9 | Construction du DataFrame | `pd.DataFrame(records)` | Transformation finale des données API en DataFrame Pandas. |
| 10 | Limitation requêtes API | `time.sleep(0.05)` | Pause entre les requêtes pour éviter le blocage par l’API. |

---

# Comparaison des valeurs manquantes — Table WDI

| Colonne                     | Valeurs manquantes AVANT | Valeurs manquantes APRÈS |
|----------------------------|--------------------------|---------------------------|
| country_code               | 0                        | 0                         |
| country                    | 0                        | 0                         |
| year                       | 0                        | 0                         |
| population_total           | 30                       | 0                         |
| population_active          | 30                       | 0                         |
| population_elderly         | 30                       | 0                         |
| female_active_population   | 8760                     | 0                         |
| female_population_pct      | 0                        | 0                         |
| gdp_total                  | 2587                     | 0                         |
| gdp_per_capita             | 2587                     | 0                         |
| gdp_growth                 | 3014                     | 0                         |
| urban_population_pct       | 114                      | 0                         |
| urban_density              | 6384                     | 0                         |
| population_density         | 1607                     | 0                         |

## Conclusion
- Plusieurs indicateurs macro-économiques et démographiques présentaient des valeurs manquantes importantes.
- Les colonnes les plus impactées étaient :
  - female_active_population
  - urban_density
  - gdp_growth
  - population_density
- Après le nettoyage, toutes les valeurs manquantes ont été traitées.
- Le dataset final est entièrement exploitable pour l’analyse et la visualisation.




## Téléchargement Automatique de Tous les Indicateurs WDI

Ce bloc de code permet de télécharger **automatiquement l’ensemble des indicateurs socio-économiques** définis dans le dictionnaire `indicators` en utilisant l’API de la **Banque Mondiale (World Bank API)**.

### Fonctionnement du Processus

| Étape | Action | Description |
|-------|--------|-------------|
| 1 | Initialisation | Création d’une liste vide `frames` destinée à stocker temporairement les DataFrames de chaque indicateur. |
| 2 | Boucle de parcours | La boucle `for name, code in indicators.items()` parcourt tous les indicateurs un par un. |
| 3 | Affichage du suivi | La fonction `print()` permet d’afficher en temps réel l’indicateur en cours de téléchargement. |
| 4 | Appel API | La fonction `get_wdi_indicator(code, name)` interroge l’API World Bank pour extraire les données. |
| 5 | Vérification des données | La condition `if not df_temp.empty` vérifie si les données récupérées ne sont pas vides. |
| 6 | Stockage temporaire | Si les données existent, elles sont ajoutées à la liste `frames` via `frames.append(df_temp)`. |
| 7 | Gestion des erreurs | Si aucune donnée n’est trouvée, un message d’alerte est affiché (`Vide → indicateur`). |
| 8 | Validation du téléchargement | Le nombre total de lignes récupérées est affiché pour chaque indicateur.

---

### Objectif de cette Étape

✔ Automatise la collecte de toutes les données WDI  
✔ Évite le téléchargement manuel indicateur par indicateur  
✔ Assure le contrôle qualité des données extraites  
✔ Prépare les données pour la phase de **fusion multi-indicateurs**



## Fusion Progressive des Indicateurs Socio-Économiques (WDI)

Ce bloc de code permet de **fusionner progressivement tous les DataFrames des indicateurs WDI** téléchargés précédemment en un seul DataFrame global appelé `df_wdi`.

### Principe de Fonctionnement

| Étape | Action | Description |
|-------|--------|-------------|
| 1 | Initialisation | Le premier DataFrame des indicateurs est utilisé comme base avec `df_wdi = frames[0]`. |
| 2 | Parcours des indicateurs | La boucle `for f in frames[1:]:` parcourt tous les autres DataFrames restants. |
| 3 | Fusion progressive | La fonction `merge()` permet d’ajouter chaque indicateur au DataFrame principal. |
| 4 | Clés de jointure | La fusion est réalisée sur les colonnes communes : `country`, `country_code` et `year`. |
| 5 | Type de jointure | Le paramètre `how="outer"` garantit que **toutes les données sont conservées**, même si certaines valeurs manquent. |
| 6 | Construction finale | Le DataFrame `df_wdi` devient un tableau complet contenant l’ensemble des indicateurs économiques, démographiques et urbains. |

---

### Objectif de cette Étape

✔ Centraliser tous les indicateurs dans une seule table  
✔ Conserver l’historique complet des pays et des années  
✔ Préparer les données pour le nettoyage avancé  
✔ Faciliter l’analyse croisée avec les données sportives (JO)



## Tableau des Modifications – Calcul des KPI Actionnables

| Nom du KPI | Nouvelle Colonne Créée | Colonnes Utilisées | Formule Appliquée | Objectif |
|------------|--------------------------|---------------------|-------------------|----------|
| Performance pondérée par million | `kpi_weighted_per_million` | `medal_weighted`, `population_total` | `medal_weighted / (population_total / 1e6)` | Comparer la performance indépendamment de la taille de la population |
| Médailles par PIB total | `kpi_medals_per_gdp` | `total_medals`, `gdp_total` | `total_medals / gdp_total` | Mesurer l’efficacité économique |
| Médailles par PIB/habitant | `kpi_medals_per_gdp_capita` | `total_medals`, `gdp_per_capita` | `total_medals / gdp_per_capita` | Performance relative au niveau de vie |
| Médailles par population active | `kpi_medals_per_active_pop` | `total_medals`, `population_active` | `total_medals / population_active` | Efficacité de la population active |
| Score d’efficacité active | `kpi_active_efficiency_score` | `medal_weighted`, `population_active` | `medal_weighted / population_active` | Performance sportive de la force de travail |
| Taux de participation | `kpi_participation_rate` | `athletes`, `population_active` | `athletes / population_active` | Mesure l’implication sportive du pays |
| Participation féminine estimée | `kpi_female_participation_active` | `female_population_pct`, `athletes` | `(female_population_pct × athletes) / 100` | Estimation de la participation des femmes |
| Efficacité composite globale | `kpi_efficiency_composite` | `medal_weighted`, `population_total`, `gdp_per_capita` | `medal_weighted / (population_total × gdp_per_capita)` | Performance globale population + économie |
| Performance par milliard $ PIB | `kpi_weighted_per_gdp` | `medal_weighted`, `gdp_total` | `medal_weighted / (gdp_total / 1e9)` | Normalisation économique forte |
| Médailles par croissance du PIB | `kpi_medals_per_gdp_growth` | `total_medals`, `gdp_growth` | `total_medals / gdp_growth` | Impact de la croissance économique |
| Médailles & urbanisation | `kpi_medals_urbanization` | `total_medals`, `urban_population_pct` | `total_medals / urban_population_pct` | Effet de l’urbanisation |
| Médailles par densité population | `kpi_medals_density` | `total_medals`, `population_density` | `total_medals / population_density` | Performance par densité humaine |
| Médailles par densité urbaine | `kpi_medals_urban_density` | `total_medals`, `urban_density` | `total_medals / urban_density` | Impact de la concentration urbaine |



### Nettoyage Final des KPI

Cette étape supprime toutes les lignes contenant au moins une valeur manquante (`NaN`) dans le DataFrame `df_kpi`.

L’objectif est de garantir que l’ensemble des indicateurs calculés est **complet, cohérent et directement exploitable** pour les analyses statistiques, les visualisations et les tableaux de bord.(python:  df_kpi = df_kpi.dropna()) 
    
| Ligne de code | Action | Objectif |
|---------------|--------|----------|
| `df_kpi = df_kpi.merge(noc[["NOC", "country"]], on="country", how="left")` | Fusion des dataframes | Ajouter la colonne `NOC` à `df_kpi` à partir de `noc` selon la colonne `country` |
| `df["NOC"] = df["NOC"].replace("SGP","SIN")` | Remplacement de valeur | Normaliser le code pays `"SGP"` en `"SIN"` dans le dataframe `df` |
| `df_kpi.to_csv("df_kpi.csv", index=False)` | Export CSV | Sauvegarder le dataframe `df_kpi` dans un fichier CSV |
| `noc.to_csv("df_noc.csv", index=False)` | Export CSV | Sauvegarder le dataframe `noc` dans un fichier CSV |
| `df.to_csv("df_ath.csv", index=False)` | Export CSV | Sauvegarder le dataframe `df` dans un fichier CSV |
| `df_final = df_final.drop("NOC_y", axis=1)` | Suppression de colonne | Supprimer la colonne `NOC_y` pour éviter les doublons après fusion |


# Connextion avec SQL

| Ligne de code | Action | Objectif |
|---------------|--------|----------|
| `from sqlalchemy import create_engine` | Import de la fonction `create_engine` | Permet de créer un moteur de connexion à une base SQL depuis Python |
| `engine = create_engine("mysql+pymysql://root:Hiba41hh07%40@localhost:3306/olympiade", echo=False)` | Création du moteur de connexion | Se connecter à la base MySQL `olympiade` avec l’utilisateur `root` et le mot de passe fourni |
| `noc.to_sql("dim_region", engine, if_exists="replace", index=False)` | Insertion DataFrame `noc` | Créer ou remplacer la table `dim_region` dans MySQL avec les données de `noc` |
| `df.to_sql("dim_athlete", engine, if_exists="replace", index=False)` | Insertion DataFrame `df` | Créer ou remplacer la table `dim_athlete` dans MySQL avec les données de `df` |
| `df_final.to_sql("dim_kpi", engine, if_exists="replace", index=False)` | Insertion DataFrame `df_final` | Créer ou remplacer la table `dim_kpi` dans MySQL avec les données de `df_final` |

# Creation de database
| Ligne de code | Action | Objectif |
|---------------|--------|----------|
| `DROP DATABASE IF EXISTS olympiade;` | Suppression de la base si elle existe | Nettoyer l’environnement avant de créer une nouvelle base |
| `CREATE DATABASE olympiade;` | Création de la base de données | Créer une nouvelle base MySQL nommée `olympiade` |
| `USE olympiade;` | Sélection de la base | Définir la base `olympiade` comme base active pour les commandes suivantes |
| `ALTER TABLE olympiade.dim_kpi ADD COLUMN PK INT NOT NULL AUTO_INCREMENT PRIMARY KEY;` | Ajout d’une colonne `PK` | Créer une clé primaire auto-incrémentée pour `dim_kpi` |
| `ALTER TABLE olympiade.dim_athlete ADD COLUMN PK INT NOT NULL AUTO_INCREMENT PRIMARY KEY;` | Ajout d’une colonne `PK` | Créer une clé primaire auto-incrémentée pour `dim_athlete` |
| `ALTER TABLE dim_region ADD PRIMARY KEY (NOC);` | Définition clé primaire | Définir `NOC` comme clé primaire de `dim_region` |
| `ALTER TABLE dim_kpi ADD FOREIGN KEY (NOC_x) REFERENCES dim_region(NOC);` | Ajout clé étrangère | Relier `dim_kpi` à `dim_region` via `NOC_x` |
| `ALTER TABLE dim_athlete ADD FOREIGN KEY (NOC) REFERENCES dim_region(NOC);` | Ajout clé étrangère | Relier `dim_athlete` à `dim_region` via `NOC` |

# shema relationelle
<img width="621" height="556" alt="image" src="https://github.com/user-attachments/assets/0b52d65b-8fe9-40a2-afb5-1a7c74212fe4" />





# Modelisation avec tableau
<img width="1025" height="494" alt="image" src="https://github.com/user-attachments/assets/f20084f5-ab00-4065-bf4b-83fb2c377faa" />


<img width="863" height="483" alt="image" src="https://github.com/user-attachments/assets/df0d4fb7-1ade-45b8-9a9c-40e39f298947" />

<img width="1011" height="541" alt="image" src="https://github.com/user-attachments/assets/16803ebc-e4e5-4d2c-a314-3afbb7148cc1" />

<img width="1045" height="507" alt="image" src="https://github.com/user-attachments/assets/b22c5de6-8126-4ed6-b92f-a8482e636839" />


<img width="1037" height="491" alt="image" src="https://github.com/user-attachments/assets/dc6a961e-fe08-4ea5-8150-c7ab2517cc18" />

<img width="1035" height="515" alt="image" src="https://github.com/user-attachments/assets/634b9bd0-2739-48a4-8c4a-4e560d2378b7" />

<img width="1154" height="575" alt="image" src="https://github.com/user-attachments/assets/2b5a71c8-0661-4954-8f12-e1d980bc1c83" />

<img width="1124" height="549" alt="image" src="https://github.com/user-attachments/assets/b1e366c5-a139-4ee1-ab58-f563bead6a4c" />

<img width="1049" height="547" alt="image" src="https://github.com/user-attachments/assets/d03a24b0-b95a-4342-848f-3d41a9805cae" />

<img width="787" height="474" alt="image" src="https://github.com/user-attachments/assets/b53fb3e1-46c7-4daf-8304-1a4948446fb4" />






















   









