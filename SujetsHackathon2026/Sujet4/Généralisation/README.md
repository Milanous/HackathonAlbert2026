## QUESTIONS

--- 
### 📌 QUESTIONS POUR LA GÉNÉRALISATION : DÉTECTION AVANCÉE DE LA FLOTTE FANTÔME

#### **🔹 Objectif**
Concevoir un pipeline automatisé pour détecter le spoofing AIS à grande échelle.
Utiliser les fichiers :
- `ships_large.csv` (1 000 navires)
- `ais_data_large.csv` (10 000 données AIS)
- `suspicious_behaviors_large.csv` (500 comportements suspects)
- `risk_zones_large.csv` (50 zones à risque)
- `alerts_large.csv` (200 alertes)

--- 

#### **📊 Partie 1 : Nettoyage et Prétraitement des Données**
1. **Nettoyage des données AIS**
   - Détectez et supprimez les **doublons** dans `ais_data_large.csv` (même `mmsi` + même `timestamp`).
   - Combien de doublons avez-vous supprimés ?
   - Remplacez les valeurs manquantes (ex: `destination`) par "Unknown".

2. **Normalisation des champs**
   - Normalisez le champ `status` pour qu'il ne contienne que 3 valeurs : "Under Way", "At Anchor", "Moored".
   - Convertissez les champs `timestamp` en format `datetime` et ajoutez une colonne `hour_of_day`.

3. **Enrichissement des données**
   - Ajoutez une colonne `is_in_risk_zone` dans `ais_data_large.csv` pour indiquer si la position (`latitude`, `longitude`) se trouve dans une zone à risque (voir `risk_zones_large.csv`).
   - Combien de points AIS sont dans une zone à risque ?

--- 

#### **🔍 Partie 2 : Détection des Comportements Suspects**
4. **Détection des désactivations d'AIS**
   - Identifiez les navires avec **AIS désactivé pendant plus de 24h consécutives** dans `ais_data_large.csv`.
   - Combien de navires sont concernés ?
   - Générez une alerte pour chaque cas (format : `ALERT-AIS-OFF-{mmsi}`).

5. **Détection de spoofing de MMSI**
   - Trouvez les lignes dans `ais_data_large.csv` où le `mmsi` **n'existe pas** dans `ships_large.csv` (MMSI falsifiés).
   - Combien de MMSI falsifiés avez-vous détectés ?
   - Associez chaque MMSI falsifié à un navire suspect (via `suspicious_behaviors_large.csv`).

6. **Détection de fausses positions**
   - Pour chaque navire, calculez la **distance** entre ses positions AIS consécutives (utilisez `geopy.distance.geodesic`).
   - Détectez les **sauts de position anormaux** (> 100 km en 1h).
   - Combien de sauts anormaux avez-vous détectés ?

7. **Détection de vitesses anormales**
   - Identifiez les lignes dans `ais_data_large.csv` où la `speed` > 30 nœuds (limite physique pour la plupart des navires commerciaux).
   - Combien de fois une vitesse anormale a-t-elle été détectée ?
   - Quels navires sont les plus souvent concernés ?

8. **Détection de changements de cap brutaux**
   - Pour chaque navire, calculez la **différence de cap** (`course`) entre ses points AIS consécutifs.
   - Détectez les **changements de cap > 90° en moins de 10 minutes**.
   - Combien de changements brutaux avez-vous détectés ?

--- 

#### **📈 Partie 3 : Analyse des Zones à Risque**
9. **Statistiques par zone à risque**
   - Pour chaque zone dans `risk_zones_large.csv`, calculez :
     - Le **nombre de navires** y ayant été détectés.
     - Le **nombre de comportements suspects** (via `suspicious_behaviors_large.csv`).
   - Quelle zone a le **plus grand nombre de comportements suspects** ?

10. **Carte des zones à risque** (optionnel)
    - Utilisez `folium` pour créer une carte interactive montrant :
      - Les **zones à risque** (colorées par `risk_level`).
      - Les **positions des navires suspects** (marqueurs rouges).
      - Les **alertes actives** (marqueurs oranges).

--- 

#### **🤖 Partie 4 : Modélisation et Automatisation**
11. **Graphe de connaissances**
    - Construisez un **graphe de connaissances** (avec `networkx` ou Neo4j) pour modéliser les relations entre :
      - Navires (`ships_large.csv`)
      - Comportements suspects (`suspicious_behaviors_large.csv`)
      - Zones à risque (`risk_zones_large.csv`)
      - Alertes (`alerts_large.csv`)
    - Quels sont les **navires les plus connectés** (degré le plus élevé) ?

12. **Scoring de risque global**
    - Calculez un **score de risque global** pour chaque navire en combinant :
      - `risk_score` (depuis `ships_large.csv`)
      - Nombre de **comportements suspects** associés.
      - Nombre de **passages en zone à risque**.
      - Nombre d'**alertes ouvertes**.
    - Classez les navires par score de risque (Top 10).

13. **Pipeline d'alertes automatisées**
    - Écrivez un script Python qui :
      1. Surveille en temps réel les nouvelles données AIS (simulées).
      2. Détecte les **comportements suspects** (AIS désactivé, spoofing, etc.).
      3. Génère une **alerte** dans `alerts_large.csv` si un comportement est détecté.
      4. Envoie un **email** (simulé) ou génère un **rapport PDF** pour les alertes critiques.
    - Testez votre pipeline sur 100 nouvelles données AIS simulées.

14. **Optimisation des performances**
    - Mesurez le **temps d'exécution** de votre pipeline pour 10 000 données AIS.
    - Proposez des optimisations pour réduire ce temps (ex: parallélisation, indexing).

--- 

#### **📌 Consignes pour les Réponses**
- **Format des réponses** : Utilisez `pandas`, `networkx`, `geopy`, et `folium` pour répondre aux questions.
- **Exemple de réponse** :
  ```python
  import pandas as pd
  from geopy.distance import geodesic
  
  ais_data = pd.read_csv("ais_data_large.csv")
  ais_data["timestamp"] = pd.to_datetime(ais_data["timestamp"])
  ais_data = ais_data.sort_values(["mmsi", "timestamp"])
  
  # Détecter les sauts de position > 100 km
  ais_data["prev_lat"] = ais_data.groupby("mmsi")["latitude"].shift(1)
  ais_data["prev_lon"] = ais_data.groupby("mmsi")["longitude"].shift(1)
  ais_data["prev_timestamp"] = ais_data.groupby("mmsi")["timestamp"].shift(1)
  
  def calculate_distance(row):
      if pd.isna(row["prev_lat"]):
          return None
      time_diff = (row["timestamp"] - row["prev_timestamp"]).total_seconds() / 3600  # en heures
      if time_diff > 1:  # Seuil de 1h
          return geodesic((row["prev_lat"], row["prev_lon"]), (row["latitude"], row["longitude"])).km
      return None
  
  ais_data["distance_km"] = ais_data.apply(calculate_distance, axis=1)
  jumps = ais_data[ais_data["distance_km"] > 100]
  print(f"Nombre de sauts anormaux : {len(jumps)}")
  ```
- **Livrables** :
  - Un fichier `reponses_generalisation_flotte_fantome.py` avec le code.
  - Un rapport `rapport_generalisation_flotte_fantome.md` avec les résultats et analyses.

