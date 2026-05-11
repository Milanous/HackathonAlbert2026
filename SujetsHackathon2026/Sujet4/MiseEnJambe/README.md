## QUESTIONS

--- 
### 📌 QUESTIONS POUR LA MISE EN JAMBE : DÉTECTION DE LA FLOTTE FANTÔME

#### **🔹 Objectif**
Comprendre les bases du spoofing AIS et identifier des comportements suspects simples.
Utiliser les fichiers :
- `ais_data_small.csv` (50 données AIS)
- `suspicious_behaviors_small.csv` (20 comportements suspects)
- `risk_zones_small.csv` (10 zones à risque)

--- 

#### **📊 Partie 1 : Exploration des Données AIS**
1. **Structure des données AIS**
   - Quels sont les champs présents dans `ais_data_small.csv` ?
   - Quel est le type de chaque champ (ex: `mmsi` = String, `latitude` = Float) ?

2. **Navires avec AIS désactivé**
   - Combien de lignes dans `ais_data_small.csv` ont `ais_active = False` ?
   - Listez les MMSI des navires ayant désactivé leur AIS.

3. **Comportements suspects**
   - Combien de comportements suspects sont enregistrés dans `suspicious_behaviors_small.csv` ?
   - Quel est le comportement suspect le plus fréquent ?

4. **Zones à risque**
   - Combien de zones à risque sont définies dans `risk_zones_small.csv` ?
   - Quelles zones ont un niveau de risque "Critical" ?

--- 

#### **🔍 Partie 2 : Détection de Comportements Suspects**
5. **Association navires → comportements suspects**
   - Pour chaque navire dans `suspicious_behaviors_small.csv`, trouvez ses données AIS dans `ais_data_small.csv`.
   - Combien de navires suspects ont **au moins un comportement suspect** enregistré ?

6. **Détection de spoofing de MMSI**
   - Identifiez les lignes dans `ais_data_small.csv` où le `mmsi` commence par "FAKE-".
   - Combien de fois un MMSI falsifié a-t-il été détecté ?

7. **Écarts de position**
   - Pour chaque navire, comparez ses positions AIS dans le temps.
   - Détectez les navires avec des **sauts de position anormaux** (écart > 0.05° entre deux points consécutifs).

8. **Croiser avec les zones à risque**
   - Pour chaque ligne dans `ais_data_small.csv`, vérifiez si la position (`latitude`, `longitude`) se trouve dans une zone à risque de `risk_zones_small.csv`.
   - Combien de fois un navire est-il passé dans une zone à risque "Critical" ?

--- 

#### **📈 Partie 3 : Analyse et Visualisation**
9. **Statistiques descriptives**
   - Calculez la **moyenne**, **médiane**, et **écart-type** de la vitesse (`speed`) des navires.
   - Tracez un histogramme des vitesses.

10. **Carte des positions suspectes** (optionnel)
    - Utilisez `folium` pour tracer une carte des positions des navires avec AIS désactivé ou des comportements suspects.
    - Ajoutez des marqueurs pour les zones à risque.

--- 

#### **💡 Partie 4 : Questions Ouvertes**
11. **Hypothèses**
    - Pourquoi un navire désactiverait-il son AIS ?
    - Quels sont les risques associés au spoofing de MMSI ?

12. **Améliorations possibles**
    - Quels champs manquent dans `ais_data_small.csv` pour une détection plus efficace des comportements suspects ?
    - Comment pourrait-on **automatiser** la détection des écarts de position ?

--- 

#### **📌 Consignes pour les Réponses**
- **Format des réponses** : Utilisez `pandas` pour répondre aux questions 1 à 10.
- **Exemple de réponse** :
  ```python
  import pandas as pd
  ais_data = pd.read_csv("ais_data_small.csv")
  ais_disabled = ais_data[~ais_data["ais_active"]]
  print(f"Nombre de lignes avec AIS désactivé : {len(ais_disabled)}")
  ```
- **Livrable** : Un fichier `reponses_mise_en_jambe_flotte_fantome.py`.

