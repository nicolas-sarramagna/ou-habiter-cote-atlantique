## Contexte de construction
La construction s'est faite à partie d'un backlog initial, centré sur les points principaux que sont une recherche efficace et un affichage exploitable des points.

La construction itérative a fait émerger de nouveaux besoins et de nouvelles idées dont certains n'ont pas été implémentés ou intégrés à la solution.
En effet, la cible était de disposer d'un App opérationnelle permettant de donner des résultats concrets suffisamment pertinents et exploitables.
Ainsi, certaines US n'ont pas été prises car :
 - elles avaitent une faible Businness Value
 - l'effort de mise en oeuvre et/ou d'intégration était jugé trop important
 - le ROI était trop faible
 - les données étaient indisponibles ou demandaient trop d'effort de collecte/analyse/mise en forme
 

### Projet Source
Stack technique : Eclipse, Java, principales libs : scraping avec Jsoup et de quoi manipuler du json : jackson, twitter4j + commons-lang/io

En terme d'organisation projet, je suis sur un package par critère avec un *main* qui sort un fichier csv pour le critère à intégrer dans la WebApp

Le process général d'un *main* est :
 - phase 1 : scraping i.e récupération brute des données
 - phase 2 : extraction des données et écriture dans un fichier csv de travail dédié  
On itère les phases 1 et 2 jusqu'à l'obtention d'un fichier de travail satisfaisant.
 - phase 3 : alimentation des latitudes et longitudes non renseignées, par appel d'API Maps ou à la main en cas d'échec (vérification Google + recherche manuelle dans Maps)
 Il arrive également qu'à cette phase qu'on fasse des corrections manuelles d'adresse.  
 On écrit en sorite dans un nouveau fichier csv qui sera celui cible.
 On itère la phase 3 jusqu'au remplissage complet des latitudes et longitudes.
  - phase 4 : vérification programmatiques des latitudes et des longitudes : bornes min et max considérées comme acceptables sinon un Warning pour analyse manuelle est levé  
  - phase 5 : génération des éléments du critère sur une carte OpenStreetMap statique dans un fichier Html pour vérification visuelle de présence et de dispersion des points.  
 En cas de Warning de la phase 4, le point apparaît en général hors du périmètre établi, l'appel API de récupération des lat/lon n'a pas été pertinent, il faut redresser les données à la main avec Google + Google Maps.
 En phase 5, par exemple, si en analyse visuelle on voit très peu de généralistes en Vendée, il faut vérifier si c'est réellement le cas ou si on n'a pas récupéré toutes les données.  
 On itère les phases 4 et 5 pour avoir aucun Warning en pahse 4 et une analyse Métier satisfaisante en phase 5.
 A ce stade, on dispose du fichier de sortie cvs qui servira d'input en partie Back de la Web App.

 
 


Test de manipulation de fichiers geojson avec la lib de https://github.com/ngageoint/simple-features-geojson-java mais ca tient pas la volumétrie et décevant en durée de traitement, j'ai procédé autrement, je le détaille plus bas.

Tips, retours d'expérience et conseils en scraping :
  TODO




### Projet Back - services REST
Stack technique : Eclipse, Java 8, Spring Boot (start Web) avec Maven avec twitter4j + commons-lang/io

Tips, retours d'expérience et conseils en scraping :
  TODO
  
- exposition de 2 Web Service REST
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

### Projet Front - interface Web
Stack technique : Eclipse, Html5, CSS, Javasccript avec libs Bootstrap, Jquery, Leaflet + plugins 

Interface divisée en 4 parties :
- zone de recherche
- zone de resulats avec la carte
- zone de résultats au format texte pour le détail d'un point
- zone d'historique des recherches

Tips, retours d'expérience et conseils en scraping :
  TODO
