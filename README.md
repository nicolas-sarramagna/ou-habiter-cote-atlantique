# ou-habiter-cote-atlantique
Application Web pour déterminer les zones géographiques préférentielles d’habitation selon critères

# En cours de rédaction au 29/07/2020

## Objectif du projet
L'objectif est de déterminer *finement* sur la carte de France pour les départements 64, 40, 33, 17, 85 et 44 les lieux qui correspondent aux critères sélectionnés.
Les résultats correspondent à des points de 500m de diamètre sur un maximum de 30km du bord de côte.En résultats,

Ainsi, il convient de déterminer, des ~ 67000 points de la carte prénsentée ci-dessous, quels sont ceux qui correpondent le mieux à mes critères.

![image of the map all points](/screenshots/screenshot_map_all_points.png "Map avec tous les points de la carte")

Zoom sur la carte pour visualiser précisément un ensemble de points :
![image of the map all points](/screenshots/screenshot_map_all_points_detail_example.png "Zoom sur la map des points de résultat")

## Présentation 
Ce repo a pour but de partager des éléments techniques et fonctionnels de la Web App construite.

Démo gif ci-dessous :
  1. Sélection des critères
  2. Sélection du taux de score demandé

## Côté technique
3 projets sous Eclipse
  - 1 projet **Source** dédié à la collecte des données et leur mise en forme, projet non lié à la Web App.
  - 1 projet **Back** Web App backend qui sert des services REST
  - 1 projet **Front** Web App frontend Javascript pour l'interface
  
### Projet Source
Stack technique : Java, principales libs : scraping avec Jsoup et de quoi manipuler du json : jackson, twitter4j + commons-lang/io

Test de manipulation de fichiers geojson avec la lib de https://github.com/ngageoint/simple-features-geojson-java mais ca tient pas la volumétrie, j'ai procédé autrement, je le détaille plus bas.

Tips, retours d'expérience et conseils en scraping :
  TODO

Un package par critère avec un *main* qui sort un fichier csv pour le critère à intégrer dans la WebApp


### Projet Back
- Web Service REST
```Java
	// retourne les points de resultat et leur score
	@CrossOrigin (origins = "*")
	@RequestMapping (value = "/algofiltre-score", method = RequestMethod.POST)
	public ResponseEntity<Resultat> buildMapResultScore (
			@RequestParam (value = "scoreMini", required = false, defaultValue = "100") int scoreMini,
			@RequestBody List<CritereMinimal> cList) throws Exception {
      
  // retourne les elements pour un point
	@CrossOrigin (origins = "*")
	@RequestMapping (value = "/algodetail-extra", method = RequestMethod.POST)
	public ResponseEntity<ResultatDetail> getCriteresFromPointIdExtra (@RequestParam int pointId,
			@RequestBody List<CritereMinimal> cList) throws Exception {
```

### Projet Front

## Côté fonctionnel
Liste des 35 critères intégrés à date. 
Evidemment, tous les critères souhaités et souhaitables n'ont pas été intégrés et en résultat, des vérifications manuelles et de terrains sont à effectuer.
 Domaine :
  - Loisirs : critères Plage, Cinéma, Piscine, Médiathèque, Salle de Sport
  - Santé : critères pédiatre, généraliste, pharmacie, gynécologue, hopital, maternité (de niveau 3 pour mes besoins de recherche), dermatologue, ophtalmologue
  - Education : critères maternelle, primaire, collège, lycée, études supérieures
  - Services de proximité : critères bureau de poste, agence Société Générale (contexte de ma recherche encore), marché, supermarché (carrefour city et contact, petit casino, 8 à 8, monoprix, simply market, spar, vival)
  - Services : hypermarché (auchan, carrefour, carrefour market, geant casino, intermarche, leclerc, super casino, super u), bricolage (brico depot, bricomarche, bricorama, castorama, leroy merlin), Décathlon, ameublement (but, conforama, la cas, ikea), Ikea (critère spécifique bien que compris dans le criète ameublement, créé pour le contexte de ma recherche), fnac (fnac, cultura)
  - Transports : critères aéroport, gare
  - Immobilier : prix/m2 et anciennes ventes avec prix actualisé, données issues de Etalab, base DVF notamment
  
  
  

