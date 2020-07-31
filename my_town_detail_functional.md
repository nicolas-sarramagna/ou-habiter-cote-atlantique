## Contexte

Il convient de déterminer des ~ 67000 points de la carte présentée ci-dessous, quels sont ceux qui correpondent le mieux aux critères choisis.

![image of the map all points](/screenshots/screenshot_map_all_points.png "Map avec tous les points de la carte")

Zoom sur la carte pour visualiser précisément un ensemble de points :
![image of the map all points](/screenshots/screenshot_map_all_points_detail_example.png "Zoom sur la map des points de résultat")

## Critères mis en place

  - Loisirs : plage, cinéma, piscine, médiathèque, salle de sport
  - Santé : pédiatre, généraliste, pharmacie, gynécologue, hopital, maternité (de niveau 3 dans le cadre du projet), dermatologue, ophtalmologue
  - Education : maternelle, primaire, collège, lycée, études supérieures
  - Services de proximité : bureau de poste, agence Société Générale (contexte projet), marché, supermarché (carrefour city et contact, petit casino, 8 à 8, monoprix, simply market, spar, vival)
  - Services : hypermarché (auchan, carrefour, carrefour market, geant casino, intermarche, leclerc, super casino, super u), bricolage (brico depot, bricomarche, bricorama, castorama, leroy merlin), Décathlon, ameublement (but, conforama, la cas, ikea), Ikea (critère spécifique bien que déjà compris dans le critère ameublement, critère Ikea créé pour le contexte de ma recherche), culture (fnac, cultura)
  - Transports : aéroport, gare
  - Immobilier : prix/m2 et anciennes ventes avec prix actualisé, données issues de Etalab, base DVF notamment
   
Evidemment, tous les critères souhaités ([géorisques](https://www.georisques.gouv.fr/) par exemple) et souhaitables n'ont pas été intégrés.

Un filtrage supplémentaire manuel sera donc à effectuer en résultats de recherche.  
Sur un objectif d'achat immobilier, la validation du secteur sur place m'apparaît également plus que recommandée.

## Fonctionnalités rendues

### Recherche
  - sélection des critères à utiliser en recherche (mini 1)  
  Ex : boulangerie
  - choix pour chaque critère de la distance maximale (intervalle de distance possible prédéfini dans le fichier html)    
  Ex : boulangerie à moins de 800m
  - choix pour chaque critère du nombre d'éléments voulus  
  Ex : 2 boulangeries à moins de 800m
  - choix du mode de recherche : 
    - recherche *stricte* : 100% des critères doivent être respectés
    - recherche *relachée* : au moins 90% ou 80% des critères doivent être respectés. Dans ce mode, il est également possible de spécifier des critères qui doivent être respectés à 100%  
 - affichage du nombre de points de résultat, dont ceux qui correspondent à 100%
 - lors d'une recherche *relachée*, affichage du score directement dans le point (affichage effectif selon le nombre total de points sinon problème de performance)
 - couleur différenciée en dégradé des points de la carte pour visualiser rapidement les différences de score des points
 - performance en temps de recherche et d'affichage permettant l'affinage des critères par interrogation multiple de la fonction de recherche
 
### Résultats sur la carte
 - positionnement de l'affichage permettant une visualisation globale des résultats
 - fonction clic & zoom sur un point de carte permettant de se positionner directement dans la zone géographique du point de résultat
 - sur les points de résultat, affichage paramétrable en nombre et en choix des éléments des critères  
 Ex : je n'affiche que les 2 plus proches éléments et que ceux du domaine Santé et du critère culture
 - export pdf de la carte, avec échelle, selon le niveau de zoom courant  
 Cela permet d'inspecter sur le terrain les points de résultat et globalement d'échanger sur les zones géographiques des points de résultat 
 - possibilité de basculer vers un rendu Google Maps (pour affichage des Points d'Intérêt Maps par ex) et un rendu Google Satellite (pour une meilleure visibilité terrain des points)

### Détails textuels complémentaires d'un point de résultat
Sur clic d'un point de la carte,  il est affiché, en correspondant avec les critères choisis :
- la description des éléments pour chaque critère  
Ex : nom, prénom et adresse pour les généralistes  
Ex : nom du magasin et adresse pour les hypermarchés
- la mise en exergue des résultats qui ne correpondent pas à 100% des critères

### Historique des recherches
 - l'historique des recheches perdure après la fermeture du navigateur. Les anciennes recherchent sont effacées en cas de suppression d'historique du navigateur (utilisation du localstorage)
- possibilité de charger une ancienne recherche 
- suppression ciblée, globale des anciennes recherches
- affichage des paramètres des anciennes recherches

## Utilisation
L'application Web est démarrée et s'utilise, via un navigateur Web tel que Chrome, sur le pc portable du développeur.


