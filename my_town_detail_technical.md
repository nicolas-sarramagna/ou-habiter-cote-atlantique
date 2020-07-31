## Contexte de construction
La construction s'est faite à partir d'un backlog initial, centré sur les US principales que sont :
- un jeu de données minimal
- un principe de recherche efficient et efficace avec une première implémentation 
- un carte des points visuellement exploitable

En effort global, on est sur une répartition de l'ordre d'environ 65% sur la partie 'Source', ~15% pour la partie back et ~20% sur la partie front.

### Mise en service
L'application Web se démarre sous Eclipse avec une config de JVM par défaut, issue du .ini :
 - XX:MaxPermSize=256m
 - Xms256m
 - Xmx1024m
 
et tourne sur un pc portable personnel, sans exigences matérielles spécifiques.

### Projet Source
Stack technique : Eclipse, Java, principales libs : scraping avec Jsoup et de quoi manipuler du json : jackson, twitter4j + commons-lang/io

En terme d'organisation projet, je suis sur un package par critère avec un *main* qui sort un fichier csv pour le critère à intégrer dans la WebApp

Le process général d'un *main* pour un critère est :
 - phase 1 : scraping i.e récupération brute des données
 - phase 2 : extraction des données et écriture dans un fichier csv de travail dédié  
On itère les phases 1 et 2 jusqu'à l'obtention d'un fichier de travail satisfaisant.
 - phase 3 : alimentation des latitudes et longitudes non renseignées, par appel d'API Maps (api OpenStreeetMap bien moins efficace) ou à la main en cas d'échec (vérification Google + recherche manuelle dans Maps).  
 Il arrive également qu'à cette phase l'on fasse des corrections manuelles d'adresse.  
 On écrit en sortie dans un nouveau fichier csv qui sera celui cible.
 On itère la phase 3 jusqu'au remplissage complet des latitudes et des longitudes.
  - phase 4 : vérification programmatique des latitudes et des longitudes : bornes min et max considérées comme acceptable sinon un Warning pour analyse manuelle est levé  
  - phase 5 : génération des éléments collectées sur une carte OpenStreetMap statique dans un fichier Html pour vérification visuelle de présence et de dispersion des points.  
 En cas de Warning de la phase 4, le point apparaît en général fortement hors du périmètre courant, l'appel API de récupération des lat/lon n'a pas été pertinent, il faut alors redresser les données à la main avec Google + Google Maps.  
 En phase 5, si en analyse visuelle on voit, par exemple, très peu de médecins généralistes en Vendée, il faut vérifier si c'est réellement le cas ou si on n'a pas un raté sur la phase de collecte ou si la source est incomplète.  
 On itère les phases 4 et 5 pour n'avoir aucun Warning (phase 4) et une analyse Métier satisfaisante (phase 5).    
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
  - librairie [Jsoup](https://jsoup.org/) simple et efficace (avec gestion des cookies pour les sources qui le nécessitent)
  - randomiser de quelques secondes les appels de scraping en init et sur les appels suivants
  - sur limitation à cause d'un nombre maximal d'appels, changer plusieurs fois d'ip avec [VPNBook](https://www.vpnbook.com/) par exemple et étaler la collecte sur plusieurs jours. Coder également un scraping itératif qui reprend sur les prochains liens/éléments non déjà collectés.
  - rajouter en header un user-agent usuel de navigateur/tablette/smartphone et le referrer utilisé sur le site constituent une bonne pratique
  - ne pas oublier l'open data qui donne directement les données sous un format, normalement directement exploitable.
  - analyser entièrement le code source de la page Web à scraper. En effet, il arrive régulèrement que les données recherchées soient directement disponibles sous forme de lien vers un fichier json complet ou que ce dernier soit déclaré dans une variable javscript dans le code Html !
  - certains sites bloquent tout robot, passer alors par une sauvegarde html via le navigateur. Automatiser (via plugin navigateur ad-hoc) ou pas (selon votre estimation de ROI) en variabilisant le tout si le site le permet. Parser ensuite les fichiers avec jsoup pour l'extraction des données.
  
  #### Les sources de données, indications
  - Loisirs : plage, cinéma, piscine, médiathèque, salle de sport
  - Santé : après travaux sur les données de l'annuaire d'Ameli et les données [data.gouv.fr, annuaire santé de la Cnam](https://www.data.gouv.fr/fr/datasets/annuaire-sante-de-la-cnam/), j'ai noté (avec Maps !) qu'on était loin d'avoir l'exhaustivité des professionnels de santé. Je recommande l'annuaire du [conseil national de l'Odre des médecins](https://www.conseil-national.medecin.fr/annuaire)  
  - Education : data.gouv.fr et data.education.gouv.fr
  - Services de proximité : bureau de poste, agence Société Générale (contexte projet), marché, supermarché (carrefour city et contact, petit casino, 8 à 8, monoprix, simply market, spar, vival)
  - Services : hypermarché (auchan, carrefour, carrefour market, geant casino, intermarche, leclerc, super casino, super u), bricolage (brico depot, bricomarche, bricorama, castorama, leroy merlin), Décathlon, ameublement (but, conforama, la cas, ikea), Ikea , culture (fnac, cultura)
  - Transports : aéroport, gare
  - Immobilier : Etalab, [base DVF](https://www.data.gouv.fr/fr/datasets/demandes-de-valeurs-foncieres-geolocalisees), notamment

  
### Projet Back - services REST
Stack technique : Eclipse, Java 8, Spring Boot (starter Web) avec Maven et twitter4j + commons-lang/io. Pas de base de données.
 
 - au démarrage de l'application, une phase d'init est opérée pendant laquelle on charge la liste des points possibles (~67k) et tous les éléments des critères (~77k au total). On construit, *par critère*, 2 TreeMaps, une pour les latitudes des éléments, l'autre pour les longitudes des éléments.
 - de facon générale, la recherche consiste à itérer sur chaque point possible et sur chaque critère afin de déterminer le nombre d'éléments qui matchent.  
 Cela permet de calculer le score du point et de déterminer ainsi si l'on conserve ou non le point en résultat de recherche.  
 En itération sur le critère, on calcule à partir des coordonnées du point en cours de traitement et de la distance maximale souhaitée sur ce critère les bordures min et max en lat et lon. On fait l'intersection des éléments récupérés des TreeeMap lat et lon du critère avec ces paramètres de bordures.  
 De ces coupes *horizontale* (en lat) et *verticale* (en lon), on affine le résultat, notamment pour les éléments  qui se trouvent aux pointes afin de garantir la distance maximale voulue et pour garantir la distance minimale demnandée.  
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
```
Le WS **/algofiltre-score** prend en entrée une liste de critères et le score minimum demandé.  
De façon générale, un élément de la liste des inputs aura un nom + valeur min en mètre + valeur max en mètre + nombre d'éléments + isMandatory : true/false.    
Ce WS retourne la liste des points avec id, lat, lon et score.
```Java
// retourne les elements pour un point
@CrossOrigin (origins = "*")
@RequestMapping (value = "/algodetail-extra", method = RequestMethod.POST)
public ResponseEntity<ResultatDetail> getCriteresFromPointIdExtra (@RequestParam int pointId,
			@RequestBody List<CritereMinimal> cList) throws Exception {
```
Le WS **/algofiltre-extra** prend en entrée un id de point et retourne la liste des éléments des critères avec pour chacun lat, lon, une description, et la distance en mètres vis à vis du point.

- l'application tourne et est utilisée en local sur le poste de dev : pas de process de déploiement ou de principes de sécurité particuliers mis en oeuvre.

### Projet Front - interface Web
Stack technique : Eclipse, Html5, CSS, Javascript avec libs Bootstrap, Jquery, Leaflet + plugins 

Interface divisée en 4 parties, cf copie d'écran ci-dessous scalée à 80% pour bien voir les 4 zones :
![image of the interface 4 zones](/screenshots/screenshot_map_4_zones_80.png "Interface avec les 4 zones")

- zone de résulats avec la carte
	- passage de la map Leaflet en canvas pour un affichage fluide sur beaucoup de points de résultat
	- utilisation de la lib js pour Leaflet [L.Control.Layers.Tree](https://github.com/jjimenezshaw/Leaflet.Control.Layers.Tree) pour les filtres par critères ou par domaine de critères sur la map
	- utilisation de la lib js pour Leaflet [leaflet.icon-material](https://github.com/ilyankou/Leaflet.IconMaterial) pour l'affichage des icônes des éléments
	- utilisation de la lib js pour Leaflet [L.Control.ZoomMin](https://github.com/alanshaw/leaflet-zoom-min) pour le bouton supplémentaire sur la map permettant de revenir au zoom global d'un clic
	- utilisation de la lib js pour Leaflet [leaflet.browser.print](https://github.com/Igor-Vladyka/leaflet.browser.print) pour l'export pdf de la carte
	- positionnement du score dans les markers Leaflet, lors d'une recherche avec score < 100% permis, que pour un nombre de résultats < 1000 sinon performance d'interaction dégradée ; utilisation du bindTooltip à améliorer (la MR Leaflet suivante [https://github.com/Leaflet/Leaflet/issues/6734)](https://github.com/Leaflet/Leaflet/issues/6734) pourrait aider).
	
- zone de résultats au format texte pour le détail d'un point : format des description fait côté back

- zone de recherche : sliders construits avec la lib js [ion.rangeSlider](https://github.com/IonDen/ion.rangeSlider)

- zone d'historique des recherches : stockage dans le localstrore au format Json des paramètres des recheches effectuées

- appels à l'api REST via Jquery

- manipulation de la logique Front avec Jquery et pur Javascript, en ordre d'idée, cela donne ~850 lignes de code JS, commentaires et indentation comprises.

Côté fonctionnel, c'est sur ce lien : [my_town_detail_functional.md](/my_town_detail_functional.md "détails fonctionnels sur le projet")
