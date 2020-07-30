# ou-habiter-cote-atlantique
Application Web pour déterminer les zones géographiques préférentielles d’habitation selon critères

# En cours de rédaction au 29/07/2020

## Objectif du projet
L'objectif est de déterminer *finement* sur la carte de France pour les départements 64, 40, 33, 17, 85 et 44 les lieux qui correspondent aux critères sélectionnés.
Les résultats correspondent à des points de 500m de diamètre sur un maximum de 30km du bord de côte.

Ainsi, il convient de déterminer, des ~ 67000 points de la carte prénsentée ci-dessous, quels sont ceux qui correpondent le mieux à mes critères.

![image of the map all points](/screenshots/screenshot_map_all_points.png "Map avec tous les points de la carte")

Zoom sur la carte pour visualiser précisément un ensemble de points :
![image of the map all points](/screenshots/screenshot_map_all_points_detail_example.png "Zoom sur la map des points de résultat")

## Présentation 
Ce repo a pour but de partager des éléments techniques et fonctionnels de la Web App construite.

TODO Démo gif ci-dessous :
  1. Sélection des critères
  2. Sélection du taux de score à 90% pour l'exemple
  3. Affichage des résultats
  4. Zoom sur un point
  5. Affichage détail d'un point

## Côté fonctionnel
35 critères ont été intégrés à date. 
Evidemment, tous les critères souhaités ([géorisques](https://www.georisques.gouv.fr/) par exemple) et souhaitables n'ont pas été intégrés.
D'un résultat, un filtre supplémentaire manuelle et de terrains sont à effectuer.

Les détails se trouvents dans le fichier [my_town_detail_functional.md](/my_town_detail_functional.md "détails fonctionnels sur le projet")

## Côté technique
3 projets sous Eclipse
  - 1 projet **Source** dédié à la collecte des données et leur mise en forme, projet non lié à la Web App.
  - 1 projet **Back** Web App backend qui sert des services REST
  - 1 projet **Front** Web App frontend Javascript pour l'interface
  
Les détails se trouvents dans le fichier [my_town_detail_technical.md](/my_town_detail_technical.md "détails techniques sur le projet")
  



