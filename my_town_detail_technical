### Projet Source
Stack technique : Eclipse, Java, principales libs : scraping avec Jsoup et de quoi manipuler du json : jackson, twitter4j + commons-lang/io

Test de manipulation de fichiers geojson avec la lib de https://github.com/ngageoint/simple-features-geojson-java mais ca tient pas la volumétrie, j'ai procédé autrement, je le détaille plus bas.

Tips, retours d'expérience et conseils en scraping :
  TODO

Un package par critère avec un *main* qui sort un fichier csv pour le critère à intégrer dans la WebApp


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
