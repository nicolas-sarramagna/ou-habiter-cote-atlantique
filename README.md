# ou-habiter-cote-atlantique
Application Web pour déterminer les zones géographiques préférentielles d’habitation selon critères.

# En cours de rédaction 07/2020

## Objectif
Mon besoin était de déterminer *finement* sur la carte de France pour les départements 64, 40, 33, 17, 85 et 44 les lieux qui correspondent à des critères sélectionnés.

Les résultats correspondent à des points de 500m de diamètre sur un maximum de 30km du bord de côte, ce qui correspond à ma distance maximale souhaitable.

## Présentation 
Ce repo a pour but de partager des éléments fonctionnels et techniques (difficultés rencontrées, recommandations, détails de mise en oeuvre) de la Web App construite.

Si vous avez les mêmes besoins, vous aurez un retour d'expérience pour votre mise en oeuvre.

Si vous souhaitez échanger sur ce sujet, [contactez-moi](https://www.linkedin.com/in/nicolas-sarramagna) ! :)

Déroulé de la Démo gif ci-dessous :

TODO lien gif

Cette démo présente les éléments suivants :
1. recherche sur respect à 100% des critères :
  - présentation de criteres disponibles
  - actions possibles sur les critères
  - chargement d'une ancienne recherche
  - modidications sur les critères
  - lancement de la recherche
  - clic sur un point de résultat
  - présentation d'icônes affichés
  - présentation de la partie textuelle d'un point de résultat
  - changement des paramètres d'affichage (carte + texte) du nombre d'éléments d'un point de résultat
  - clic retour vision large de la carte
  - changement du paramètrè de présence des éléments sur la carte
  - clic sur un autre point de résultat
2. recherche avec respect minimal de 80% des critères
  - lancement de la recherche
  - clic sur un point de score < 100%
  - présentation des éléments textuels non matchés
  - clic sur export de la carte
      
## Côté fonctionnel
35 critères ont été intégrés à date. 

Les détails se trouvent dans le fichier [my_town_detail_functional.md](/my_town_detail_functional.md "détails fonctionnels sur le projet")

## Côté technique
3 parties :
  - 1 partie **Source**, dédiée à la collecte des données et leur mise en forme, projet non lié à la Web App.
  - 1 partie **Back**, pour la Web App côté backend qui sert des services REST
  - 1 partie **Front**, pour la Web App côté frontend en Javascript pour l'interface Web
  
Les détails se trouvent dans le fichier [my_town_detail_technical.md](/my_town_detail_technical.md "détails techniques sur le projet")
  



