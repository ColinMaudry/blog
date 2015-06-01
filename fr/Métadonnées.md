# L'unification des données et métadonnées en entreprise, ou la fin des silos

## Problème : des formats et des technologies hétérogènes

Les métadonnées sont des données qui décrivent des objets d'information. Les plus connues sont les métadonnées ID3 pour les MP3 (artiste, album, etc.) et les métadonnées EXIF pour les photos (temps de pose, réglages ISO, etc.).

Ces métadonnées ne sont malheureusement pas exprimées dans un format et un langage communs et standards. Il est nécessaire d'appliquer un traitement spécifique à chaque format de fichier ce qui, dans une organisation de grande taille, peut devenir une tâche titanesque et hasardeuse. Les systèmes de gestion de contenu (SharePoint, Alfresco, etc.) proposent d'uniformiser la gestion des métadonnées des fichiers, mais ces systèmes implémentent chacun leur technologie de gestion et leur langage de métadonnées. Cela complique la comparaison des métadonnées extraites de systèmes concurrents. 

Quant aux objets d'information tels que les produits, les clients ou les fournisseurs, leurs données sont le plus souvent gérées dans des silos distincts non-interopérables (SAP, Oracle, ENOVIA, etc.).

## La solution : un format dédié et standard

La solution consiste à utiliser un format et des langages standards pour gérer les métadonnées hétérogènes. Le W3C, l'organisation qui a standardisé HTML, CSS et XML, a également publié un tel standard, baptisé RDF (Resource Description Framework). De nombreuses solutions open source et performantes sont basées dessus, du stockage à la visualisation (graphiques en tous genres et interfaces de navigation).

Tout en gardant les systèmes de gestion existants, la solution consiste à établir des exports périodiques depuis ces systèmes au format RDF et d'alimenter une base RDF qui rassemble les métadonnées de l'entreprise. Ainsi, il est possible d'interroger cette base et de recueillir des informations sur tous les types d'objets dans une même requête. Par exemple :

"Quels fichiers de documentation, non traduits en chinois, traitent de produits vendus à des clients chinois et mis en vente depuis janvier 2015 ?" ⇒  On obtient ainsi la liste des fichiers à faire traduire en chinois en priorité.

Plus ambitieux : il est également possible d'établir une synchronisation dans les deux sens, ce qui permet non seulement de consulter les métadonnées dans leur ensemble grâce aux exports périodiques, mais également de répercuter les modifications faites dans la base RDF dans le système d'origine. 

## Une solution éprouvée

Cela fait deux ans qu'NXP Semiconductors utilise RDF pour simplifier la gestion de sa documentation et de ses produits. Facebook, Google, Wikipedia, la BBC et de nombreuses autres entreprises ont franchi le pas depuis longtemps.