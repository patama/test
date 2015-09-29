# Guide de bonnes pratiques API

## De la conception des APIS à l'exposition contrôlée des ressources

### Introduction

Ce document cherche à recenser les principes directeurs associés à la conception, au développement et à l’exposition d’API dans le contexte de l’Etat Plateforme.
Il se veut être un guide sur le chemin de l’ouverture des SI à travers les interfaces de type API en excluant les autres types d’interactions entre systèmes, par exemple les échanges asynchrone de fichiers.
Ce document s’inspire très largement de plusieurs sources publiques accessibles via l’internet et citées ci-dessous.
Ce document est le « point de vue initial » du Service Architecture et Urbanisation de la DISIC au moment du lancement du hackathon « Etat Plateforme France Connect » qui verra la naissance de services à destination des utilisateurs utilisant les APIs des SI des administrations centrales et territoriales.
Il a été rédigé dans l’objectif de provoquer des réactions : compléments, questions, autres points de vue. 
Sur la base de ce document et des réactions qu’il aura suscitées sera élaboré le document final « guide des bonnes pratiques autour des API » qui s’inscrira dans le Cadre d’Architecture de l’Etat Plateforme.

Ce document s’appuie sur les sources suivantes :

- Un article[^f1] du blog de Mulesoft, éditeur de solutions, résumant les bonnes pratiques API  [1] ;
- Une page[^f2] du dépôt de code des standards API de la Maison Blanche [2] ;
- Une page[^f3] du site gouvernemental anglais Global Delivery Service [3];
- Une page[^f4] du blog d’Octo, cabinet de conseil IT, synthétisant les bonnes pratiques de conception et de design d’API REST [4] ;
- Une page[^f5] du blog de Vinay Sahni [5];
- Le référentiel général d’interopérabilité[^f6], publié par la DISIC [6].

[^f1]: http://blogs.mulesoft.org/api-best-practices-wrap-up/

[^f2]: https://github.com/WhiteHouse/api-standards

[^f3]: https://www.gov.uk/service-manual/making-software/apis.html

[^f4]: http://blog.octo.com/designer-une-api-rest/

[^f5]: http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api

[^f6]: https://references.modernisation.gouv.fr/interoperabilite
 

Les principes communs à ces différentes sources sont repris et synthétisés.  Les idées qui n’apparaissent que dans une des sources ne sont pas systématiquement reprises dans ce guide.

Pour chaque principe, on s’attachera à le décrire puis à présenter des exemples à suivre et à ne pas suivre afin de rendre le concept manipulé le plus opérationnel possible.

Ce document est un simple guide, l’implémentation de ces principes est seulement conseillée.  

## Familles et principes

Le périmètre du document se limite aux bonnes pratiques de conception et d’exposition d’API ou services « REST ».
L’ensemble des principes présentés ici sont regroupés en 5+1 grandes familles :

- Concevoir les APIs comme des services à offrir
- Organiser l’exposition des ressources
- Choisir les actions possibles sur ces ressources
- Offrir plusieurs formats de réponses
- Spécifier et documenter les APIs
- Hors catégories

### Concevoir les APIs comme des services à offrir
**Concevoir l’API en pensant d’abord aux usages** et en les transformant en actions sur des ressources et pas seulement en pensant aux ressources accessibles en « simple » CRUD. (Réf. :[1], [3], [4], [5])

- Qui va utiliser mon service,
- Que veut faire le client de mon service
- En tant que fournisseur d’API, que peut et que doit faire le client avec MES ressources
- Comprendre ce que les consommateurs attendent, leur offrir un service simple, clair, intuitif et compréhensible. Approche « KISS »
- Ne pas concevoir l’API seulement par rapport aux données exposées, penser aux usages.

**Planifier et concevoir son API pour les développeurs** :
Ecouter les besoins des développeurs. Un des moyens de faire est d’être soi-même le premier consommateur de son API, par exemple en construisant un site Web qui utilise les ressources exposées.(Réf. : [1], [3], [4], [5])
Se poser les questions suivantes pour chaque ressource en adoptant le point de vue du consommateur et celui du producteur : 

* Quoi exposer
* A qui est destinée la ressource,
* Pourquoi l’exposer,
* Pour en faire quoi,
* Quel risque à l’exposer
* Quelle action sur cette ressource est nécessaire, optionnelle, porteuse de valeur ou dangereuse
* …

Bon et mauvais exemples de conception d’API ([4]) : 

[![](http://blog.octo.com/wp-content/uploads/2014/10/blender_01.png)](http://blog.octo.com/wp-content/uploads/2014/10/blender_01.png)
Rechercher la simplicité et s’affranchir des données sous-jacentes


### Organiser l’exposition des ressources
Le style d’architecture REST et la philosophie RESTful imposent le découpage en **ressources** et la manipulation de ces ressources à travers les **verbes d’action** du protocole http.  La principale difficulté dans la conception de l’API réside dans les choix de découpage de l’exposition des ressources.  Les notions de [clean url](https://en.wikipedia.org/w/index.php?title=clean_url&redirect=no) ou [semantic url](https://en.wikipedia.org/w/index.php?title=semantic_url&redirect=no) permettent d’améliorer l’utilisabilité de l’API. 
Ainsi, pour accéder aux ressources, il convient d’utiliser **des noms plutôt que des verbes** et **des noms au pluriel plutôt qu’au singulier** (Réf. : [1], [2], [3], [4],[5]). 
L’utilisation de noms oriente vers un usage de type CRUD, tandis que l’utilisation de verbes oriente vers un usage de type RPC. 
L’utilisation du pluriel permet d’homogénéiser l’interface : l’URL n’est jamais modifiée que l’appel concerne une instance ou une collection.

Exemples :
Collection de ressources : 
```
/v1/users
```
Instance d’une ressource : 
```
/v1/users/007
```
Mauvais exemples de nom au singulier  ([2]) :
```
http://www.example.gov/magazine
http://www.example.gov/magazine/1234
http://www.example.gov/publisher/magazine/1234
```
Mauvais exemple de verbe ([2]) :
```
http://www.example.gov/magazine/1234/create
```

**Choisir une casse** (Réf. : [4], [5]) et si tenir parmi les choix suivants. 
On pourra choisir entre les casses suivantes : 
UpperCamelCase, lowerCamelCase, snake_case et spinal-case.
On peut faire un choix de casse différent pour décrire les ressources dans les URLs et pour peupler le corps des requêtes/réponses (http body).

Trouver le bon **niveau de granularité** dans les ressources exposées (Réf. : [4], [5]) :

* Ne pas forcément suivre le rapport théorique 1 ressource = 1 URL
* Regrouper les données qui sont systématiquement appelées ensemble
* Se limiter à deux niveaux d’imbrication : 

```
/v1/users/addresses/countries
```
### Choisir les actions possibles sur ces ressources

Les principes clés du style d’architecture REST, rappelés [ici](https://fr.wikipedia.ork/wiki/representational_state_transfer), conduisent à organiser l’API en différentes **ressources** logiques. Chaque ressource est identifiée unitairement grâce à une **URL** et on manipule les ressources grâce aux **verbes d’action** du protocole http.
On utilisera préférentiellement les 4 verbes POST, GET, PUT, DELETE et éventuellement PATCH, bien qu’il soit moins bien défini et compris.
On s’assurera que les requêtes GET sont « [safe](http://w3.org/2001/tag/doc/whentotuseget.html) » et que toutes les actions qui entrainent un changement d’état de la ressource sont réalisées à travers POST, PUT et DELETE (Réf. : [1], [2], [3], [4], [5]).

Chaque action a un effet particulier sur la ressource qu’elle manipule :

Exemples ([5]) :
`GET /tickets` retourne une liste de tickets
`GET /tickets/12` retourne le ticket #12 (s’il existe)
`POST /tickets` crée un nouveau ticket (et retourne son identifiant)
`PUT /tickets/12` met à jour le ticket #12 
`PATCH /tickets/12` met à jour une propriété du ticket #12
`DELETE /tickets/12` supprime le ticket #12 

Exemples ([2]) :

| HTTP Method | POST   | GET  | PUT    | DELETE |
| ----------- |--------| -----|--------|--------|
| CRUD OP     | CREATE | READ | UPDATE | DELETE |
| ```/dogs``` | Create new dogs | List dogs | Bulk update | Delete all dogs |
| ```/dogs/1234``` | Error | 	Show Bo | 	If exists, update Bo; If not, error |	Delete Bo |




Utiliser les **query string** comme des paramètres :
Pour gérer la notion de réponse partielle : par exemple pour modifier le niveau de détail de la réponse rendue : ```?detail=full``` ou ```?detail=none```  (Réf : [3], [4])
Ou encore pour proposer de la pagination, du tri, du filtrage et de la limitation du nombre d’enregistrements retournés (Réf : [2], [4], [5])
Par exemple, pour proposer un système de pagination simple avec 2 paramètres dans la query string : limit et offset. Si les paramètres ne sont pas indiqués par le client, limiter par défaut le nombre d’objets dans la collection. Dans la réponse, indiquez en plus du résultat, le nombre total d’enregistrements et rappelez les paramètres d’entrée :


Exemple ([2]) :
Query : 
```
GET http://example.gov/magazines?limit=25&offset=50
```
Response :
```
{
    "metadata": {
        "resultset": {
            "count": 227,
            "offset": 50,
            "limit": 25
        }
    },
    "results": […]
}
```

Pour les tris par ordre ascendant ou descendant, on utilisera la query string ``sort```: par exemple ([5]) pour retourner la liste des tickets, dans l’ordre descendant des priorités : en commençant par priorité 3 et terminant par priorité 1, puis à l’intérieur d’un niveau de priorité, dans l’ordre ascendant d’ancienneté : le plus ancien d’abord. 
```
GET /tickets?sort=-priority,created_at
```
Ou encore pour proposer à la fois des fonctions de tri et de pagination, on peut utiliser les query ```sort``` et ```range``` et les http headers ```Content-Range``` et ```Accept-Range``. Par exemple ([4]): requête des 5 premiers restaurants chinese trié par rating descendant.
```
CURL –X GET \
-H "Accept: application/json" \
https://api.fakecompany.com/v1/restaurants?type=chinese&sort=rating,name&desc=rating&range=0-4
< 206 Partial Content
< Content-Range: 0-4/12
< Accept-Range: restaurants 50
```

**Versions** :
Les APIs doivent naturellement être versionnées, ne jamais exposer une API sans version :
La version de l’API doit apparaitre dans l’URL, n’indiquer que la version majeure.
Les bonnes pratiques de gestion de version dépassent le cadre de ce document, on peut cependant indiquer :
Maintenir en production la version précédente, 
Essayer de ne pas maintenir plus de 2 versions simultanément,
Avertir les consommateurs de la publication d’une nouvelle version,
Communiquer sur les évolutions et les conséquences à prendre en compte dans leurs développements,
Indiquer les dates de fin de support des versions précédentes. 
Réf : [2], [4], [5]

L’**année** ne doit pas apparaitre dans le chemin de l’URL car c’est un filtre de recherche. Il doit aller dans la partie Query String, dans les champs de type paramètres de la recherche au même titre que l’ordre de tri.
Exemple ([2]) :
```
http://www.example.gov/magazines/2011/desc
```
### Offrir plusieurs formats de réponses
Les **ressources** peuvent être présentées sous différentes formes ou **représentations** et lorsque c’est pertinent, il faut présenter les ressources sous plusieurs formes. Au moins l’une des formes doit être lisible par les humains, tandis que les autres seront adaptés en fonction des traitements envisagés et en fonction du contenu de la ressource. Ainsi, pour des ressources très structurées, destinées à être importées en base de données, on proposera un format type csv.
Les formats les plus répandus sont :

- XML
- JSON, format adapté aux traitements
- JSONP et/ou JSON avec CORS, si l’API doit donner accès à des ressources situées dans un autre domaine, les navigateurs récents supportent le standard CORS. Si l’on souhaite être compatible avec les anciens navigateurs (IE 7à9), il faut utiliser JSONP et passer par les tags <\script /> (Réf : [2], [4], [5])
- CSV, pour importer dans un tableur ou une BDD
- ATOM, pour les flux d’évènements
Il existe également des formats spécifiques à un type de ressources, par exemple iCalendar, vCard, KML, geoRSS ou encore m3u.

Exemple d’exposition d’une ressource sous forme de 2 représentations (Réf : [2],[3],[6]) :

```
http://example.gov/api/v1/magazines.json
http://example.gov/api/v1/magazines.xml
```

La [tendance actuelle](http://www.google.com/trends/explore?q=xml+api#q=xml%20api%20%2C%20json%20api&cmpt=q) privilégie le format json au format xml pur ([5]):

<img><script type="text/javascript" src="//www.google.com/trends/embed.js?hl=en-US&q=xml+api+,+json+api&cmpt=q&tz=Etc/GMT-2&content=1&cid=TIMESERIES_GRAPH_0&export=5&w=500&h=330"></script></img>

**Négociation du format des représentations** (Réf : [1], [2], [3], [4], [5]) :
La balise « content type header » permet de caractériser le format de la ressource présentée.
La balise « accept » dans le header http permet d’engager avec le consommateur une phase de de négociation avant de lui présenter la ressource au format demandé.


Utiliser les « **http Status Codes** » lors du dialogue entre client et serveur pour les codes erreur http (Réf : [1], [2], [4], [5]) :
On peut dans un premier temps se limiter à 3 ces codes :
200 – OK : Achèvement sans erreur
400 - Bad Request : Erreur coté client
500 - Internal Server Error : Erreur coté serveur
Puis utiliser des codes supplémentaires dans chaque famille :
201, 202, 204, 206
401, 403, 404, 405, 406, 410, 415, 422, 429

__ajouter 300 ?__

Fournir également des **libellés de description d’erreur** (Réf : [1], [2],[4]) :
S’inspirer des google errors, vnd.error et json api’s error format :
En cas d’erreur, fournir en plus du http Status Code, un message à destination du développeur, un message pour le end-user (si approprié), le code erreur interne (documenté par ailleurs) et  des liens vers la documentation.

Exemple ([2]) :
```{
  "status" : 400,
  "developerMessage" : "Verbose, plain language description of the problem. Provide developers suggestions about how to solve their problems here",
  "userMessage" : "This is a message that can be passed along to end-users, if needed.",
  "errorCode" : "444444",
  "moreInfo" : "http://www.example.gov/developer/path/to/help/for/444444,
   http://drupal.org/node/444444",
}
```

**Ne jamais mettre de valeurs dans les clés** ([2]) :
Bon exemple :
```
"tags": [
  {"id": "125", "name": "Environment"},
  {"id": "834", "name": "Water Quality"}
],
```

Mauvais exemple :
```
"tags": [
  {"125": "Environment"},
  {"834": "Water Quality"}
],
```

### Spécifier et Documenter les APIs
Utiliser une approche " *Spec Driven Development* " pour créer rapidement l’API à partir de sa **spécification**, pour documenter l’API et pour faciliter sa maintenance évolutive.
Plusieurs outils émergent actuellement sur ce sujet, on peut citer par ordre d’apparition : [Swagger](http://swagger.io/), [API Bluprint](https://apiary.io/blueprint) et [RAML](http://raml.org). Chacun est soutenu par un consortium d’acteurs différents et présente des avantages et des inconvénients : voir [ici](http://www.mikestowe.com/2014/12/api-spec-comparison-tool.php) pour une comparaison des 3 outils.
La spécification à partir de ces outils permet également de proposer très facilement aux utilisateurs des « mocks » de service et affiner avec eux les interfaces et les fonctions.
Réf : [1], [2], [4], [5]

Proposer une **documentation** de l’API avec des exemples :
Possibilité d’expérimenter directement depuis la documentation.
Illustration de l’utilisation de l’API en incluant des appels cURL.
Réf : [1], [3], [4], [5]

Penser à proposer dans les réponses des liens **hypermedia** pour guider les développeurs et leur faire découvrir les ressources et actions possibles de l’API. C’est le concept HATEOAS : Hypermedia
 As The Engine Of Application State.
Réf : [1], [3], [4], [5]

Dans la documentation de l’API, spécifier également les **niveaux d’engagements** de l’API en termes de sécurité, disponibilité, limite de consommation, temps de réponse, source des données fournies, …
Ces engagements peuvent également apparaitre dans les headers des réponses données au client (http code 429 – Too many Requests) ou en utilisant, par exemple, les notations de twitter :
X-Rate-Limit-Limit
X-Rate-Limit-Remaining
X-Rate-Limit-Reset
Réf : [5]

### Hors catégories
#### SDK - bibliothèques
La fourniture de SDK ou de bibliothèques pour différents langages clients de l’API est une bonne pratique. Cependant s’ils simplifient l’usage de l’api, il faut les maintenir dans le temps et en assurer le support aux utilisateurs (Réf : [1], [3], [4], [5]). 

#### API Management
Utiliser une plateforme pour gérer l’exposition des apis : elle offre des fonctionnalités de protection, de scalabilité contre une attaque intentionnelle ou une simple mauvaise utilisation mais aussi des facilités d’autorisation, de provisionning et de throttling ([1]).

#### Réutilisation d’Apis existantes
Ne pas hésiter à réutiliser les APIs qui existent plutôt que de re-développer la fonction. ([3])
Séparer l’appel à l’API tierce du reste du code, afin de permettre notamment le changement de fournisseur.
Prévoir l’indisponibilité des APIs tierces : prévenir l’utilisateur, lui offrir une alternative dégradée.
Attention lors des tests de l’API à bien prendre en compte les contraintes associées des APIs tierces en termes d’utilisabilité, de performance ou financières
Exemples : 
API d’envoi de mail : on ne veut pas envoyer de mail lors des tests,
API de webanalytics, on ne veut pas dégrader les performances ou attendre un traitement
API qui fait payer à l’usage, attention à la facture après un test de charge
…

## 5 règles pour résumer

### Les URIs comme identifiants des ressources
###Les verbes http comme identifiants des opérations
### Les réponses http comme représentations des ressources
### Les liens hypermédia comme relations entre les ressources
### Un des header comme jeton d’authentification


![Leonard Richardson Maturity Model](http://martinfowler.com/articles/images/richardsonMaturityModel/overview.png)
Leonard Richardson Maturity Model[^f7], cité dans [7]

[^f7]: http://martinfowler.com/articles/richardsonMaturityModel.html

## Ouverture en guise de conclusion 
Le présent document a voulu synthétiser les bonnes pratiques de conception des APIs dites RESTful. Il est cependant important de noter que le monde des APIs est bien plus vaste, on peut citer a minima :
Toutes les Apis Asynchrone : de type websockets, MQTT, AMQP, Stomp, webhook ou pubsub
Les APIs qui font des orchestrations coté serveur pour améliorer l’expérience utilisateur
Les interfaces basées sur des protocoles binaires : Apache Thrift par exemple
Le développement de SDK qui peuvent permettre au client de reveni vers du RPC : fournir des librairies pour que le client consomme l’api dans son langage souvent orienté procédures
Enfin, il faut citer l’autre grande technologie permettant d’exposer des web services : le SOAP (Simple Object Access Protocol), protocole pouvant s’appuyer sur http ou une autre couche de transport.

Secrétariat général pour la modernisation de l’action publique
Direction interministérielle des systèmes d’information et de communication

Avril 2015 - Septembre 2015


Sources et Travaux Cités :

[1]: http://blogs.mulesoft.org/api-best-practices-wrap-up/

[2]: https://github.com/WhiteHouse/api-standards

[3]: https://www.gov.uk/service-manual/making-software/apis.html

[4]: http://blog.octo.com/designer-une-api-rest/

[5]: http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api

[6]: https://references.modernisation.gouv.fr/interoperabilite

[7]: http://martinfowler.com/articles/richardsonMaturityModel.html



