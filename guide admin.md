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

### Solutions pour Windows 

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

Exemple pour Firefox (qui fonctionnerait aussi avec Windows). A priori ça doit être valable pour l’ensemble des OS, dans les versions récentes de Firefox (> 24 à vérifier) :
Il faut poser un fichier xml (code ci dessous) dans `~/.mozilla/firefox/<profile>/searchplugins/` ou `C:\Users\chucknorris\AppData\Roaming\Mozilla\Firefox\Profiles\rz9azz.default\searchplugins`
Et le configurer dans `~/.mozilla/firefox/<profile>/search-metadata.json` ou `C:\Users\chucknorris\AppData\Roaming\Mozilla\Firefox\Profiles\rz9azz.default\ search-metadata.json`

```
<SearchPlugin xmlns="http://www.mozilla.org/2006/browser/search/" xmlns:os="http://a9.com/-/spec/opensearch/1.1/">
<os:ShortName>Qwant.com</os:ShortName>
<os:Description>Le moteur de recherche qui facilite la découverte et le partage grâce à une approche sociale</os:Description>
<os:InputEncoding>UTF-8</os:InputEncoding>
<os:Image width="16" height="16">data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2pJREFUeNpcU1toHFUY/s6Zmd1JZmfm7DbZJJsLmxVjm27CiEmDSjVrKQi2NhF8E5KCQVAoRhAVFJGCb1oR6UNdiJcnRTRvfVDcvNhaTMIEa1tb06Sb2zaTdWez99uMM9FI9IcDh3P+7/tv30/wP8s+fmKUlCrnUCrFuKNRBkWGXauahONmrWLhc/nrL+YO+pP9S/7EaUY83hkqq2Pl1Cay60lUfRKoxwOP2ITAscfAKwoaxv3Z2t3bZ9nlWdPF8ftgjgUSVA1odWML5k4K6WLORL2ig1JQjhv1Xl+E3D8I4YG+MaqoYQcdc0n2CKhPmaGqX6tt3kMtbbiP00Op5GcHU808b72KwPK7Vkhm/FNBjTulzOAyxknuyadHSXc0kUxbKBt/mGHLiAUWr+rfh44w1V/SXHD0JWuh+VwyVzh/WCMemqARlXEPDsPa2o5RKskT+XwZ80YN8/W2aRd8pUfTuv3ySmeQJdxjfONLro31adI7t3S7UnnPWt2BXbwO0rQxQfInz6wYdTH8Q0Y1p/RL/kvaFOvmpZVQR551PbPstJlAJEVQvmaKZ+d7CSFm4Xw4ww9KjDBR5+/IHeHtQgPrkqK76W5IqlZnQUY78ujq2AURPBCcUaJcYJWvTrklzXFdnA7bGgVsjf94YBzpTBb3MiXgJ2A2MoDudhXH62nIRptDsISR0DXAU4DNre01VAhvgcgMUEPghcGuVV+uJUxv7Gj/NFz3BJtNoghsW8oB9TDWm3fR4LfNMO7qf0+tpkG1gSZRpzQgzSk9AXBKM3v47cTk0pcT5mouM53l8ygLJZTFQ0iSFvNGgxsnx2rmd4vDk+DAIAoOk3iFZ17uoiDyk+1hhj/Xdy+cjut6MrVgLjuCstdSMVgleCJL+nOPzJvfLgxplJALkKhTh+XkEvhoT8qfXNv6tFSsv3jr1/tYaxhmxk7ieOQ2+3Dsg3+l7kZ2waxZZk+wq0CDxUloc2pPiXpi4/XIkcBQ/0OtWjVXZTlDBHEkvG9O5IxAOeaXFLSrHUDV1tG58Yq7Snte8TeHzLee7R0pmZV4a3cQPQP98Afb8OOdFybd/1bWx9oDUTBfFDWrJY7O4oijh+p/tnHf3vh58ehNM/tyLPj78KOHFoYti77vFcSTtm394vP2XjwcfO23g/5/CTAAuUtTR7QTMVYAAAAASUVORK5CYII=</os:Image>
<SearchForm>https://www.qwant.com/</SearchForm>
<os:Url type="application/x-suggestions+json" method="GET" template="https://www.qwant.com/suggest" resultDomain="qwant.com">
  <os:Param name="q" value="{searchTerms}"/>
  <os:Param name="client" value="opensearch"/>
</os:Url><os:Url type="text/html" method="GET" template="https://www.qwant.com/" resultDomain="qwant.com">
  <os:Param name="q" value="{searchTerms}"/>
  <os:Param name="client" value="opensearch"/>
</os:Url>
</SearchPlugins>
```

Par exemple :

```
{"[app]/google.xml":{"order":7,"alias":"g"},"[app]/yahoo.xml":{"order":8},"[app]/bing.xml":{"order":9},"[app]/amazondotcom.xml":{"order":10},"[app]/eBay.xml":{"order":11},"[app]/twitter.xml":{"order":6,"alias":"tw"},"[app]/wikipedia.xml":{"order":5,"alias":null,"hidden":false},"[profile]/duckduckgo.xml":{"order":12,"alias":"ddg"},"[profile]/startpage-ssl.xml":{"order":2,"alias":"st"},"[profile]/qwantcom.xml":{"order":1,"alias":"qw"},"[profile]/wikipdia-fr.xml":{"order":4,"alias":"wfr"},"[profile]/wikipedia-en-ssl.xml":{"order":3,"alias":"wen"},"[global]":{"current":"Qwant.com","hash":"m8kkXBAQxppw2+82n/B9dgEro8FKEfvhtQs2PwzCcfI="}}
```
