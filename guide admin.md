# Guide Administrateur


## Différentes possibilités
Il existe différentes solutions pour mettre Qwant :

1. Placer Qwant sur la home page intranet
  * en utilisant l'API 
  * en ajoutant une iframe 
  * voir le lien [suivant](https://www.qwant.com/as-default/) ou [celui-ci](https://www.qwant.com/extension/)
2. Mettre qwant comme moteur dans la liste
3. Ajouter en favori sur le navigateur, vu qu’on ne peut pas l’ajouter sur la homepage du navigateur à la place de l’intranet)

Ces 3 solutions dépendent des contraintes et de la stratégie.

## Ajout du moteur dans le navigateur

Ces solutions dépendent fortement du système d'explotation et du navigateur. Voici des pistes de mise en oeuvre pour quelques couples :

### SOlutions pour Windows 

#### Active Directory et GPO
Avec un Active Directory, on peut déployer via une GPO pour intégrer Qwant dans Internet Explorer :
Par exemple avec le script suivant dans un fichier.reg :

```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\SearchScopes\]
"DefaultScope"="{ED06A53F-8733-4B47-A23A-C1A404A098F3}"
[HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\SearchScopes\{ED06A53F-8733-4B47-A23A-C1A404A098F3}]
"Codepage"=dword:0000fde9
"DisplayName"="Qwant.com"
"OSDFileURL"="https://www.qwant.com/opensearch.xml"
"FaviconPath"="C:\\Users\\Ricco\\AppData\\LocalLow\\Microsoft\\Internet Explorer\\Services\\search_{ED06A53F-8733-4B47-A23A-C1A404A098F3}.ico"
"FaviconURL"="https://www.qwant.com/favicon.ico"
"SuggestionsURL_JSON"="https://api.qwant.com/api/suggest/?q={searchTerms}&client=opensearch"
"ShowSearchSuggestions"=dword:00000001
"URL"="https://www.qwant.com/?q={searchTerms}&client=opensearch"
```

Pour plus d'informations, la [documentation microsoft](https://technet.microsoft.com/fr-fr/library/Cc753092.aspx) explique comment modifier le registre windows via la GPO.

#### Sous Windows, pour tout navigateur

Créer un package MSI qui sera déployé pour les différents navigateurs avec Qwant par défaut.   

§  Dépot d'un fichier xml

### Solutions pour Mac

#### Sous Mac, pour Safari ou autre

### Solutions pour Linux

#### Sous Linux, pour Firefox ou autre

Sous Linux, il faut utiliser un déploiement via un script bash en local avec des conditions en fonction du navigateur.

