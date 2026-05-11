**Dictionnaires de données**

- ships_small_dictionary.json
- radio_signatures_small_dictionary.json
- ais_data_small_dictionary.json
- ships_large_dictionary.json
- radio_signatures_large_dictionary.json
- ais_data_large_dictionary.json
- anomalies_large_dictionary.json

Ces fichiers contiennent la structure complète des données,
avec des descriptions, des exemples, et des valeurs possibles pour chaque champ.

**Questions**
#### **🔹 Objectif**
Comprendre les données et explorer les liens entre les fichiers :
- `ships_small.csv` (navires)
- `radio_signatures_small.csv` (signatures radio)
- `ais_data_small.csv` (données AIS)

--- 

#### **📊 Partie 1 : Exploration des Données**
1. **Structure des fichiers**
   - Quels sont les champs communs entre `ships_small.csv` et `radio_signatures_small.csv` ?
   - Combien de colonnes contient chaque fichier ?

2. **Analyse des navires**
   - Quel est le navire le plus long dans `ships_small.csv` ? Quel est son MMSI ?
   - Combien de navires ont le pavillon **Panama** ?

3. **Analyse des signatures radio**
   - Quelle est la fréquence radio la plus élevée dans `radio_signatures_small.csv` ?
   - Combien de signatures radio utilisent la modulation **FM** ?

4. **Analyse des données AIS**
   - Combien de navires ont leur **AIS désactivé** (`ais_active = False`) dans `ais_data_small.csv` ?
   - Quel est le navire le plus rapide (vitesse maximale) selon les données AIS ?

--- 

#### **🔗 Partie 2 : Liens entre les Fichiers**
5. **Association navires → signatures radio**
   - Pour chaque navire dans `ships_small.csv`, trouvez sa signature radio dans `radio_signatures_small.csv` (via le champ `mmsi`).
   - Combien de navires ont **au moins une signature radio** associée ?

6. **Association navires → données AIS**
   - Combien de navires ont **au moins une donnée AIS** associée ?
   - Quels navires ont des données AIS mais **pas de signature radio** ?

7. **Anomalies simples**
   - Identifiez les navires dont la **position AIS** (`latitude`, `longitude`) ne correspond **pas** à la position de leur **signature radio** (écart > 0.001° en latitude ou longitude).
   - Combien de navires ont un **écart de position** entre AIS et radio ?

8. **Données manquantes**
   - Quels navires n\ont **ni signature radio ni données AIS** ?
   - Quels navires ont un **MMSI dupliqué** dans les fichiers ?

--- 

#### **📈 Partie 3 : Analyse et Visualisation**
9. **Statistiques descriptives**
   - Calculez la **moyenne**, **médiane**, et **écart-type** de la longueur (`length`) des navires.
   - Calculez la **moyenne** et **écart-type** de la fréquence (`frequency`) des signatures radio.

10. **Visualisation simple** (optionnel)
    - Tracez un histogramme des **vitesses** (`speed`) des navires à partir de `ais_data_small.csv`. 
    - Tracez un graphique en barres du **nombre de navires par pavillon** (`flag`) à partir de `ships_small.csv`.

--- 

#### **💡 Partie 4 : Questions Ouvertes**
11. **Hypothèses**
    - Pourquoi certains navires ont-ils leur **AIS désactivé** ? Quelles pourraient en être les raisons ?
    - Comment expliquer les **écarts de position** entre les données AIS et les signatures radio ?

12. **Améliorations possibles**
    - Quels champs manquent dans ces fichiers pour une identification **plus fiable** des navires ?
    - Comment pourrait-on **automatiser** la détection des écarts entre AIS et radio ?
