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


|valeurs avant le nettoyage| valeurs apres nettoyage|
|--------------------------|-------------------------|
|ID         |              0|          
|Name       |            0|
|Sex        |             0|
|Age        |           9474|
|Height     |         60171|
|Weight     |           62875|
|Team       |              0|
|NOC                     0|
|Games                    0|
|Year                     0|
|Season                   0|
|City                     0|
|Sport                    0|
|Event                    0|
|Medal               231333|
|dtype:               int64|                         |











   









