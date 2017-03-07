Storymap
===================

Cette application permet de valoriser des donn�es g�olocalis�es sous la forme de storymap. Il est possible d'enrichir les donn�es g�ographiques avec du contenu externe.
Il existe pour le moment 2 templates (liste et carousel) et un mode minimaliste sans template.

----------

### Principe

-------------
Pour cr�er une nouvelle storymap, il suffit de cr�er un dossier dans le r�pertoire stories et d'y d�poser les ressources n�cessaires � savoir :

**Fichiers ressources**

> - un fichier json `config.json` (obligatoire) qui contient la configuration de la storymap
> - un fichier css (optionnel) qui permet �ventiellement de styler la storymap
> - un fichier mst (optionnel) qui est un template Mustache permettant d'effectuer la mise en forme html du contenu info de la storymap
> - un fichier csv (optionnel) qui permet sur la base d'un champ commun de joindre le contenu de ce fichier aux donn�es g�ographiques de base.
> - tout autre ressource utilisable par la storymap (images...).


#### Structure du fichier `config.json`
```sh
 {
    splash :{},
    theme :{},
    tooltip :{},
    map : {},
    data : {},
    extradata: {}    
  }
```

##### splash 
- section permettant de configurer l'�cran d'accueil de l'application. Il s'agit d'un param�trage optionnel
 * prototype 
    **splash.**`iframe`: "url vers la page � utiliser" (str) 
        **ou**
    **splash.**`title`: "titre � utiliser" (str)
    **splash.**`text`: "texte � afficher" (str)

Exemple :
```
  {
  "iframe":"stories/mystory/splash.html"
  }
```
  
##### theme
 * prototype 
     **theme.**`css`: "url vers le fichier css � utliser pour personnaliser la storymap" (str).
     
Exemple :
```
  {
  "css":"mafeuilledestyle.css"
  }
```
 
##### tooltip 
- configuration des tooltips affich�s au survol de la souris sur les entit�s g�ographiques de la carte.
 * prototype :
     **tooltip.**`fields`: ["liste des champs � utiliser, s�par�s par des virgules"] (array)
     
Exemple :
```
  {
  "fields":["champ1"]
  }
```   
 
#### map 
- configuration de la carte.
 * prototype 
     **map.**`center`: ["coordonn�es (web marcator) du centre de la carte"] (array)
     
Exemple :
```
  {
  "center": [-227028,6182514]
  }
```   
 
 * prototype 
     **map.**`zoom`: "zoom (1 � 20)" (str)
 * descriptif : zoom utilis� lors de l'initialisation de la carte et du zoom sur les entit�s g�ographiques.
```
  {
  "zoom": "12"
  }
```  
 * prototype 
     **map.**`overview`: "zoom (1 � 20)" (str)
     
 * descriptif : Parmet d'afficher ou de masquer la mini carte de localisation.
 
 Exemple :
```
  {
  "overview: "true"
  }
```   
 * prototype 
     **map.**`url`: "url" (str)
     
 * descriptif : fond carto � utiliser OSM par exemple.
 
 Exemple :
```
  {
  "url": "https://{a-c}.tile.openstreetmap.org/{z}/{x}/{y}.png"
  }
```   
 
 * prototype 
     **map.**`animation`: "true" (booleen)
     
 * descriptif : Activation ou d�sactivation de l'animation de zoom lors d'un changement de focus sur les entit�s g�ographiques.
 * exemple {animation "false"}
 
#### data - configuration du contenu de la storymap.
 * prototype 
     **data.**`title`: "Titre de la story" (str)
     
  Exemple :
```
  {
  "title": "Titre de la story"
  }
```
 
 * prototype 
     **data.**`subtitle`: "Sous-titre de la story" (str)
     
 Exemple :
```
  {
  "subtitle": "En 2017"
  }
```
 
 * prototype 
     **data.**`template`: {`name`: "", `options`: {}}
     
 * descriptif : Template utilis� par la storymap au choix entre carousel et list.   
 
 Exemple :
```
  {
  "template": "carousel"
  }
```

 
 * prototype 
     **data.**`url`: "" (str)
 
 * descriptif : URl vers la source de donn�es. La source de donn�es doit �tre au format geojson avec une projection EPSG:3857.
   Il peut s'agir d'un fichier statique ou d'une flux WFS.

 Exemple 1 :
```
  {
  "url": "stories/myfirststory/data.geojson"
  }
```

 Exemple 2 :
```
  {
  "url": "http://ows.region-bretagne.fr/geoserver/rb/wfs?request=getFeature&typename=rb:reserve_naturelle_regionale&outputFormat=application/json&srsName=EPSG:3857"
  }
```

 
 * prototype 
     **data.**`id`: "" (str)
     
 * N�cessaire pour les fichiers statiques de type geojson ne poss�dant pas de propri�t� id. C'est le cas des fichiers g�n�r�s par QGIS.
 
 Exemple :
```
  {
  "id": "fid"
  }
```
 
 * prototype 
     **data.**`orderby`: "" (str)
     
 * descriptif : Ce param�tre permet de r�ordonner (ordre croissant) les entit�s g�ographiques sur la base d'un champ poss�dant des valeurs de type num�rique.   
 Via ce param�tre, il est possible de d�cider du s�quencage du contenu de la story. Le champ peut �tre pr�sent dans le fichier csv associ�.
  
 Exemple :
```
  {
  "orderby": "id"
  }
```

 
 * prototype 
     **data.**`tpl`: "" (str)
     
 * Lien vers le template Mustache � utiliser (mise ne forme html des fiches d'informations des entit�s g�ographiques). 
 
 Exemple :
```
  {
  "tpl":{tpl: "stories/camaret/camaret.mst"}
  }
```

 
 * prototype :
      **data.**`fields`: [{`name`:"", `type`:"title|text|image|url|iframe|background"}] (array)
     
 * Liste des champs � utiliser pour constituer la fiche d'information de chaque entit� g�ographique. A utiliser si le param�tre template - tpl n'est pas renseign�.
 
 Exemple :
```
  {
  "analyse":{"fields": [     {"name": "nom", "type": "title" }, 
                            {"name": "image", "type": "image"},
                            {"name": "descriptif", "type": "text"},
                            {"name": "site_web", "type": "url"},
                            {"name": "iframe", "type": "iframe"}    ]}
  }
```

#### extradata
 * prototype 
     .`extradata`: {`url`: "" (str), `linkfield`: ""} 
     
 * Param�trage n�cessaire pour joindre les donn�es contenues dans un fichier csv aux donn�es g�ographiques dur la base d'un champ commun.
 Les champs pr�sents dans le fichier csv sont ensuite disponibles pour �tre int�gr�s dans la fiche d'information.
 Deux propri�t�s sont � configurer : url pour indiquer o� se situe le fichier, linkfield pour pr�ciser le nom du fichier � utiliser pour effectuer la jointure.
 Le champ doit correspondre au champ "id" de la donn�e g�ographique.
 
 Exemple :
```
  {
  extradata: { "url": "reserve_naturelle_regionale.csv", "linkfield" : "id"}
  }
```

 
 * prototype 
     .`analyse`: {`type`: "", `field`: "", `values`: [], `styles`: {fill: {color: ""}, stroke: {color: "", width: ""}, circle:{radius: ""}, icon: {src:"", scale:""}}} 
     
 * Style unique ou analyse th�matique appliqu�es aux entit�s g�ographiques 
 
 Exemple 1 :
```
  "analyse": {
            "type": "single",
			"field": "",
			"values": [],
			"styles": [
                {
                    "icon": {
                        "src" : "stories/camaret/image/pins.svg",
                        "scale" : 0.07
                    }
                }
            ]
		}
```

 Exemple 2 :
```
  "analyse": {
            "type": "single",
			"field": "",
			"values": [],
			"styles": [
                {
                    "fill": {
                        "color": "rgba(206,227,147,0)"
                    },
                    "stroke": {
                        "color": "rgba(206,227,147,0.2)",
                        "width": 2
                    }
                }
            ]
		}
```

Exemple 3 :
```
  "analyse": {
            "type": "categories",
			"field": "mission_code",
			"values": ["territoire","formation"],
			"styles": [
                {
                    "icon": {
                        "src" : "stories/cp/image/pins_territoire.svg",
                        "scale" : 0.07
                    }
                },
                {
                    "icon": {
                        "src" : "stories/cp/image/pins_formation.svg",
                        "scale" : 0.07
                    }
                }
            ]
		}
```
 
 * prototype 
     .`hightlightstyle`: {fill: {}, stroke: {}, icon: {}}
     
 * Style unique appliqu� aux entit�s g�ographiques s�lectionn�es.
 
 Exemple :
```
  {
  "hightlightstyle":{}
  }
```