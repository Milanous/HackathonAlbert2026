## Questions

--- 
### 📌 QUESTIONS POUR LA GÉNÉRALISATION

#### **🔹 Objectif**
Automatiser l'identification passive des navires et détecter des anomalies à grande échelle.
Utiliser les fichiers :
- `ships_large.csv` (1 000 navires)
- `radio_signatures_large.csv` (5 000 signatures radio)
- `ais_data_large.csv` (10 000 données AIS)
- `anomalies_large.csv` (100 anomalies détectées)

--- 

#### **📊 Partie 1 : Création d'une Base de Données de Signatures Radio**
1. **Agrégation des signatures radio**
   - Pour chaque navire, agrégez ses signatures radio (moyenne de `frequency`, `bandwidth`, `power`, etc.).
   - Créez un fichier `ship_radio_profiles.csv` avec ces agrégats.
   - Quels sont les 5 navires avec la **fréquence moyenne la plus élevée** ?

2. **Caractéristiques uniques**
   - Identifiez les navires dont le `pulse_pattern` est unique (n'apparaît qu'une fois dans `radio_signatures_large.csv`).
   - Combien de `pulse_pattern` différents existent dans le jeu de données ?

3. **Modélisation des signatures**
   - Utilisez un algorithme de clustering (ex: K-Means) pour regrouper les navires en fonction de leurs signatures radio (`frequency`, `bandwidth`, `power`).
   - Combien de clusters obtenez-vous avec K=5 ?
   - Visualisez les clusters avec un graphique 2D (ex: `frequency` vs `power`).

--- 

#### **🔍 Partie 2 : Détection d'Anomalies**
4. **Anomalies de pavillon**
   - Identifiez les navires dont le pavillon déclaré (`flag` dans `ships_large.csv`) ne correspond **pas** à leur signature radio typique.
     Exemple : Les navires sous pavillon **Panama** ont généralement une `frequency` entre 156.8 et 157.0 MHz.
   - Générez un rapport des 10 anomalies les plus critiques (triées par `confidence`).

5. **Changements de nom**
   - Utilisez le champ `historical_names` dans `ships_large.csv` pour détecter les navires ayant changé de nom.
   - Combien de navires ont **plus de 2 noms historiques** ?
   - Associez ces changements de nom à des anomalies de type `Name Change` dans `anomalies_large.csv`.

6. **Spoofing et usurpation**
   - Détectez les signatures radio (`radio_signatures_large.csv`) qui ont un `mmsi` **non présent** dans `ships_large.csv`.
   - Combien de signatures radio sont **orphelines** (sans MMSI associé) ?
   - Proposez une méthode pour identifier le navire associé à ces signatures.

7. **AIS désactivé**
   - Identifiez les navires avec `ais_active = False` dans `ais_data_large.csv` **pendant plus de 24h consécutives**.
   - Combien de navires ont leur AIS désactivé de manière suspecte ?

8. **Écarts de position**
   - Pour chaque navire, comparez les positions AIS (`latitude`, `longitude`) et radio (`location_lat`, `location_lon`).
   - Combien de navires ont un **écart de position > 1 km** entre AIS et radio ?
   - Générez une carte (avec `folium`) montrant ces écarts.

--- 

#### **📈 Partie 3 : Analyse Temporelle et Statistique**
9. **Évolution des signatures radio**
   - Pour un navire donné (ex: `MMSI=244710000`), tracez l'évolution de sa `frequency` et de son `signal_strength` sur le temps.
   - Détectez les **changements brutaux** (ex: `frequency` passe de 156.8 à 160.0 MHz).

10. **Statistiques par pavillon**
    - Calculez la **moyenne** et l'écart-type de la `frequency` pour chaque pavillon (`flag`).
    - Quel pavillon a la **fréquence moyenne la plus élevée** ?

11. **Corrélation entre AIS et radio**
    - Calculez la corrélation entre la `speed` (AIS) et la `frequency` (radio) pour les navires.
    - Y a-t-il une corrélation significative ?

--- 

#### **🤖 Partie 4 : Automatisation et Validation**
12. **Pipeline d'identification passive**
    - Écrivez un script Python qui :
      1. Capture une **nouvelle signature radio** (simulée).
      2. L'associe à un navire en utilisant les données de `ship_radio_profiles.csv`.
      3. Détecte si la signature correspond à un **navire suspect** (via `is_suspicious`).
      4. Génère une alerte si une anomalie est détectée.

13. **Validation de la solution**
    - Testez votre pipeline sur 10 signatures radio aléatoires de `radio_signatures_large.csv`.
    - Quel est le **taux de détection correcte** (nombre de signatures correctement associées / total) ?

14. **Amélioration continue**
    - Proposez une méthode pour **mettre à jour automatiquement** la base de données de signatures radio.
    - Comment intégrer des **nouvelles données OSINT** (ex: flux RSS de navires suspects) ?

--- 

#### **📌 Consignes pour les Réponses**
- **Format des réponses** : Les réponses aux questions 1 à 14 doivent inclure :
  - Un **code Python** fonctionnel (ex: utilisation de `pandas`, `scikit-learn`, `folium`).
  - Des **visualisations** (graphiques, cartes) si nécessaire.
  - Une **analyse critique** des résultats.
- **Livrables** :
  - Un fichier `reponses_generalisation.py` avec le code.
  - Un rapport `rapport_generalisation.md` avec les résultats et analyses.

