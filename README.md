# ou-habiter-cote-atlantique
Application Web pour déterminer les zones géographiques préférentielles d’habitation selon critères.

# En cours de rédaction 07/2020

## Objectif
Mon besoin était de déterminer *finement* sur la carte de France pour les départements 64, 40, 33, 17, 85 et 44 les lieux qui correspondent à des critères sélectionnés.

Les résultats correspondent à des points de 500m de diamètre sur un maximum de 30km du bord de côte.

## Présentation 
Ce repo a pour but de partager des éléments fonctionnels et techniques de la Web App construite.

Si vous avez les mêmes besoins, vous aurez un retour d'expérience pour votre mise en oeuvre.

Si vous souhaitez creuser ce sujet, contactez-moi ! :)

TODO Démo gif ci-dessous :
  1. Sélection des critères
  2. Sélection du taux de score à 90% pour l'exemple
  3. Affichage des résultats
  4. Zoom sur un point
  5. Affichage détail d'un point

## Côté fonctionnel
35 critères ont été intégrés à date. 

Evidemment, tous les critères souhaités ([géorisques](https://www.georisques.gouv.fr/) par exemple) et souhaitables n'ont pas été intégrés.

Des résultats, pour les autres critères non pris en compte, un filtre supplémentaire manuelle sera à effectuer.  
Sur un objectif d'achat immobilier, la validation du secteur sur place m'apparaît plus que recommandé.

Les détails se trouvent dans le fichier [my_town_detail_functional.md](/my_town_detail_functional.md "détails fonctionnels sur le projet")

## Côté technique
3 projets sous Eclipse
  - 1 projet **Source** dédié à la collecte des données et leur mise en forme, projet non lié à la Web App.
  - 1 projet **Back** Web App backend qui sert des services REST
  - 1 projet **Front** Web App frontend Javascript pour l'interface
  
Les détails se trouvent dans le fichier [my_town_detail_technical.md](/my_town_detail_technical.md "détails techniques sur le projet")
  



