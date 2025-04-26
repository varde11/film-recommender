# Recommandation de films basée sur la similarité cosinus et le clustering

---

## 1. Introduction

Dans un marché du divertissement saturé, offrir des recommandations de films personnalisées est devenu un enjeu majeur pour retenir les utilisateurs et stimuler l'engagement. Les plateformes de streaming investissent massivement dans ces algorithmes, qui sont devenus des leviers essentiels de fidélisation. Par exemple, dès 2016, Netflix estimait que son moteur de recommandation lui faisait économiser plus d'un milliard de dollars par an en réduisant le taux de désabonnement.  
Face à cet enjeu, l’objectif de ce projet est de construire un système de recommandation simple, efficace et mesurable, capable de suggérer des films similaires à partir d’un film donné, tout en tenant compte de la popularité et de la qualité des films.

---

## 2. Objectif

- **Construire un système de recommandation** de films basé sur les similarités entre films.
- **Pondérer les recommandations** pour éviter les films très peu populaires ou de qualité douteuse.
- **Évaluer les performances** du système avec des métriques précises.

---

## 3. Structure du projet

- **recommendation_system.ipynb** : Notebook principal regroupant l'ensemble des étapes : chargement des données, nettoyage, ingénierie des features, construction des modèles et évaluation.

---

## 4. Dataset utilisé

- **Source** : The Movies Dataset (Kaggle)
- **Taille** : 45 466 lignes et 23 colonnes (au début du projet) ; 45 451 lignes et 83 colonnes (à la fin du porjet).
- **Caractéristiques principales** :
  - Métadonnées de films : titres, dates de sortie, genres, popularité, votes, etc.
  - Présence de valeurs manquantes et de colonnes complexes (JSON encodé en string).

---

## 5. Approche technique

### a) Nettoyage et Feature Engineering

- Conversion des colonnes JSON (genres, production_countries, production_companies, etc.) en colonnes exploitables et extraction des données sous format binaire.
- Suppression ou traitement des anomalies (budget, revenue, etc.).
- Création de nouvelles features :
  - `popularity_log`, `vote_count_log` (log-transformation pour réduire l’asymétrie des données).

### b) Construction du premier système : Similarité Cosinus

- Sélection de films selon leurs genres, pays de production, compagnies de production et langues (77 colonnes en tout).
- Construction d'une matrice de features sur les 77 colonnes.
- Calcul de la similarité cosinus entre les films.

### c) Construction d’un second système : Pondération avec le Clustering

- Clustering des films via DBSCAN basé sur `popularity_log` et `vote_average`.
- Attribution d'un cluster à chaque film.
- Lors de la recommandation, priorisation des films appartenant au même cluster que le film de référence.

---

## 6. Modèles utilisés

- **Méthode principale** : Utilisation de la similarité cosinus sur des features numériques.
- **Méthode d'amélioration** : Pondération des scores par clustering (DBSCAN).

---

## 7. Résultats

**Premier système** :  
- **MAP@k** : 88,7 %  
  *Indiquant une bonne performance pour le tout premier système de recommandation.*

**Second système** :  
- **Hit Rate** : 0.99  
  *(99 % du temps, la bonne recommandation figure dans la liste générée)*
- **Précision@K** : 0.996  
  *(Très grande précision sur les suggestions affichées aux utilisateurs)*
- **MAP@k** : 99,7 %
- **Performance du clustering DBSCAN** :  
  - Silhouette score : 0,58  
  - Davies-Bouldin score : 0,66  
  *(Révélant de bons clusters malgré une forte densité des données)*

Le système réussit ainsi à combiner pertinence et qualité des recommandations.

---

## 8. Limitations et perspectives d'amélioration

**Limites actuelles :**

- Absence d'analyse de texte sur les résumés (*overview*) pour enrichir les similarités.
- Non-prise en compte directe des préférences utilisateurs individuelles.

**Perspectives :**

- Créer une application web légère (ex : avec Streamlit) pour permettre une utilisation intuitive du système.
- Intégrer des méthodes de filtrage collaboratif basées sur des données utilisateurs réelles.
- Enrichir les similarités à l’aide du traitement de texte sur les résumés ou mots-clés (NLP).
- Optimiser le clustering en utilisant plus de colonnes (difficile à visualiser sur un schéma) .

---

## 9. Conclusion

Ce projet a permis de mettre en place un système de recommandation performant en combinant des méthodes simples mais robustes : la similarité cosinus pour identifier les films proches et le clustering pour pondérer les résultats selon leur popularité et qualité.  
Il démontre qu’avec un nettoyage rigoureux et une ingénierie des features réfléchie, il est possible de construire un moteur de recommandation compétitif même avec des ressources limitées.

---

## Point de Vue Personnel

Ce résumé offre une vision claire et structurée du projet, en mettant en avant les enjeux commerciaux et techniques d’un système de recommandation de films. J'apprécie particulièrement l'approche progressive adoptée, passant du prétraitement des données à l'implémentation de méthodes avancées comme le clustering pour optimiser la recommandation.  
Les résultats obtenus, tant en termes de MAP@k que de clustering (avec des scores de silhouette et Davies-Bouldin convaincants), témoignent d'une implémentation soignée et d'une bonne compréhension des compromis inhérents à la recommandation.  
Enfin, les perspectives d'amélioration évoquées ouvrent des pistes prometteuses pour enrichir le système, notamment via l'intégration de techniques de NLP et d'applications web interactives. Dans l'ensemble, ce projet constitue une très solide démonstration de compétences en data science et en machine learning.

