## Contexte de construction
La construction s'est faite à partir d'un backlog initial, centré sur les US principales que sont une recherche efficace et un affichage exploitable des points.

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

Le process général d'un *main* pour un critère est :
 - phase 1 : scraping i.e récupération brute des données
 - phase 2 : extraction des données et écriture dans un fichier csv de travail dédié  
On itère les phases 1 et 2 jusqu'à l'obtention d'un fichier de travail satisfaisant.
 - phase 3 : alimentation des latitudes et longitudes non renseignées, par appel d'API Maps (api OpenStreeetMap bien moins efficace) ou à la main en cas d'échec (vérification Google + recherche manuelle dans Maps)
 Il arrive également qu'à cette phase qu'on fasse des corrections manuelles d'adresse.  
 On écrit en sorite dans un nouveau fichier csv qui sera celui cible.
 On itère la phase 3 jusqu'au remplissage complet des latitudes et longitudes.
  - phase 4 : vérification programmatiques des latitudes et des longitudes : bornes min et max considérées comme acceptables sinon un Warning pour analyse manuelle est levé  
  - phase 5 : génération des éléments du critère sur une carte OpenStreetMap statique dans un fichier Html pour vérification visuelle de présence et de dispersion des points.  
 En cas de Warning de la phase 4, le point apparaît en général hors du périmètre établi, l'appel API de récupération des lat/lon n'a pas été pertinent, il faut redresser les données à la main avec Google + Google Maps.
 En phase 5, par exemple, si en analyse visuelle on voit très peu de généralistes en Vendée, il faut vérifier si c'est réellement le cas ou si on n'a pas récupéré toutes les données.  
 On itère les phases 4 et 5 pour avoir aucun Warning en pahse 4 et une analyse Métier satisfaisante en phase 5.
 A ce stade, on dispose du fichier de sortie cvs qui servira d'input en partie Back de la Web App.  
 Ce dernier comprend globalement les colonnes suivantes : 
  - département
  - nom du site ou nom et prénom d'un professionel de santé
  - adresse
  - code postal
  - ville voire en plus code insee
  - latitude
  - longitude

 #### Retours sur le scraping
  - librairie [Jsoup](https://jsoup.org/) simple et efficace (jsoup peut gérer les cookies si besoin)
  - randomiser de quelques secondes les appels de scraping en init et sur les appels suivants
  - sur limitation à cause de quota journalier maximal d'appels, changer momentanément d'ip avec [VPNBook](https://www.vpnbook.com/) par exemple.
  - rajouter en header un user-agent courant et le referrer utilisé sur le site constituent une bonne pratique
  - ne pas oublier l'open data qui donne directement les données sous un format, normalement directement exploitable.
  - analyser entièrement le code source de la page Web à scraper. En effet, il arrive régulèrement que les données soient directement disponibles sous forme de lien vers un fichier json complet ou que ce dernier soit déclaré dans une variable javscript dans le code Html !
   
### Projet Back - services REST
Stack technique : Eclipse, Java 8, Spring Boot (start Web) avec Maven avec twitter4j + commons-lang/io. Pas de bases de données.
 
 - au démarrage de l'application, une phase d'init est opérée pendant laquelle on charge la liste des points possibles (~70k) et tous les éléments des critères (~77k au total). Ces derniers sont stockés par critère, dans 2 TreeMaps, une pour les latitudes des éléments, l'autre pour les longitudes des éléments.
 - la recherche consiste à itérer sur chaque point possible et sur chaque critère afin de déterminer le nombre d'éléments qui matchent.  
 Cela permet de calculer le score du point et de déterminer ainsi si l'on conserve ou non le point en résultat de recherche.  
 En itération sur le critère, on calcule à parti des coordonnées du point en cours de traitement et de la distance maximale souhaité sur ce critère les bordures min et max en lat et lon. On fait l'intersection des éléments récupérés des TreeeMap lat et lon du critère avec ces paramètres de bordures.  
 De ces coupes *horizontale* (en lat) et *verticale* (en lon), on affine le résultat pour les éléments qui se trouvent aux pointes afin de garantir la distance maximale voulue.  
 On stoppe prématurément les calculs sur le point en cours :
  - si un critère désigné comme obligatoire (i.e doit être à 100%) ne l'est pas
  - si le score courant ne permettra pas d'atteinde le score minimum demandé (respect à mini 80% ou à mini 90% ou à 100% des critères demandés).
 
 
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
- WS /algofiltre-score prend en entrée une liste de critères et le score minimum demandé.  
De façon générale, un élément de la liste des inputs aura un nom + valeur min en mètre + valeur max en mètre + nombre d'éléments + isMandatory : true/false
En résultat de l'appel de ce WS, la liste des points (id, lat, lon, score)
- WS /algofiltre-extra prend en entrée un id de point et retourne la liste des éléments des critères avec pour chacun lat, lon, une description, et la distance en m du point demandé.

- Pas de sécurité mise en oeuvre, l'application tourne et est utilisée en local sur le poste de dev.

### Projet Front - interface Web
Stack technique : Eclipse, Html5, CSS, Javascript avec libs Bootstrap, Jquery, Leaflet + plugins 

Interface divisée en 4 parties :
- zone de recherche : sliers construits avec la lib js [ion.rangeSlider](https://github.com/IonDen/ion.rangeSlider)

- zone de resulats avec la carte
	- passage de la map Leaflet en canvas pour un affichage fluide sur beaucoup de points de résultat
	- utilisation de la lib js pour Leaflet [L.Control.Layers.Tree](https://github.com/jjimenezshaw/Leaflet.Control.Layers.Tree) pour les filtres par critères ou par domaine de critères sur la map
	TODO screenshot
	- utilisation de la lib js pour Leaflet [leaflet.icon-material](https://github.com/ilyankou/Leaflet.IconMaterial) pour l'affichage des icônes des éléments
	TODO screenshot
	- utilisation de la lib js pour Leaflet [L.Control.ZoomMin](https://github.com/alanshaw/leaflet-zoom-min) pour le bouton supplémentaire sur la map permettant de revenir au zoom global d'un clic
	- utilisation de la lib js pour Leaflet [leaflet.browser.print](https://github.com/Igor-Vladyka/leaflet.browser.print) pour l'export pdf de la carte
	- positionnement du score dans les markers Leaflet que pour un nombre de résultats < 1000 sinon performance d'interaction dégradée ; utilisation du bindTooltip à améliorer (la MR Leaflet suivante [https://github.com/Leaflet/Leaflet/issues/6734)](https://github.com/Leaflet/Leaflet/issues/6734) pourrait aider).
	
- zone de résultats au format texte pour le détail d'un point : format des description fait côté back
- zone d'historique des recherches : stockage dans le localstrore au format Json des paramètres des recheches effectuées

- appels à l'api REST via Jquery
- manipulation de la logique Front avec Jquery et pur Javascript, en ordre d'idée, cela donne ~850 lignes de code avec commentaires de JS.
