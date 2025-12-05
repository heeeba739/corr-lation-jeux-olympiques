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

| nom de fichier | bibliothéque utilisée | description |
|----------------|-----------------------|-------------|













