La Région Bretagne a fait appel à mes services de conseil et de développement pour les accompagner dans la publication des données des marchés publics bretons.

Afin de mesurer le travail à accomplir, j'ai voulu débroussailler le chemin entre les données actuelles et [le format réglementaire](https://www.data.gouv.fr/fr/datasets/referentiel-de-donnees-marches-publics/).

<!--more-->

**Mise à jour du 3 octobre** : *XSLT c'était intéressant, mais clairement pas une solution optimale. La solution optimale pour produire du JSON, c'est [jq](https://stedolan.github.io/jq/). [Suivez le guide](#avec-jq)*

Pour point de départ, j'ai opté pour [les données publiées par les collectivités bretonnes](https://breizh-sba.opendatasoft.com/explore/dataset/marches-publics-collectivites-bretonnes/table/). Ces données couvrent la période de 2013 à 2016, mais elles ont le mérite d'avoir de nombreux champs en commun avec le format réglementaire.

Cette expérience a été menée sous [Linux Mint](https://fr.wikipedia.org/wiki/Linux_Mint).

Tous les scripts, commandes et les échantillons de résultats sont regroupés sur ces Gist

- [pour la conversion avec XSLT 3.0](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0).
- [pour la conversion avec jq 1.5](https://gist.github.com/ColinMaudry/78bd2fa840b3a10225d9f4ce61a2f678)

Le fichier source de cet article est [publié sur Github](https://github.com/ColinMaudry/blog/blob/develop/fr/donn%C3%A9es-bretonnes-format-r%C3%A9glementaire.md).

## Mise en correspondance des champs ("mapping")


| Champ CSV breton               | Champ format réglementaire     | Commentaire                                                                   |
| ------------------------------ | ------------------------------ | ----------------------------------------------------------------------------- |
| Nmarché                        | id                             |                                                                               |
| SIRETMandataire                | acheteur.id                    |                                                                               |
| LibelleAcheteur                | acheteur.nom                   |                                                                               |
| Nature                         | nature                         |                                                                               |
| objet                          | objet                          | Limité à 256 caractère dans le format réglementaire                           |
| CodeCPV                        | codeCPV                        |                                                                               |
| Procedure                      | procedure                      | Besoin d'un·e expert·e métier pour mapper les valeurs :-)                     |
| CodePostalCommuneExecution     | lieuExecution.code             | Si un code INSEE et un code postal sont présents, on retiendra le code INSEE. |
| NomCommuneExecution            | lieuExecution.nom              |                                                                               |
| CodeINSEEExecution             | lieuExecution.code             | Si un code INSEE et un code postal sont présents, on retiendra le code INSEE. |
| GranulariteINSEEExecution      |                                |                                                                               |
| MillesimeMandatement           |                                |                                                                               |
| DateNotification               | dateNotification               |                                                                               |
| Montant mandate TTC            |                                |                                                                               |
| Montant mandate HT             |                                |                                                                               |
| Montant attribue TTC           |                                |                                                                               |
| Montant attribue HT            | montant                        |                                                                               |
| Date de cloture                |                                |                                                                               |
| Duree                          | dureeMois                      | Pas de données dans le CSV breton.                                            |
| SIRETContractant               | titulaires.id                  | En fait, il s'agit de numéros SIREN.                                          |
| DenominationSociale            | titulaires.denominationSociale |                                                                               |
| Role                           |                                | Les sous-traitants devront être écartés.                                      |
| CodePostal, Dpt ID, Commune... |                                | Ces données sur les titulaires pourront être récupérées via l'API SIREN.      |
|                                | datePublicationDonnees         |                                                                               |
|                                | formePrix                      | Valeurs possibles :<br/><br/>Ferme<br/>Ferme et actualisable<br/>Révisable    |
|                                | modifications                  |                                                                               |


Au final, seuls trois champs du format réglementaire ne sont pas présent dans le CSV des données bretones. Lorsque toutes les collectivités bretonnes auront rempli toutes les colonnes du format CSV breton, elles auront fait une très grande part du chemin vers la conformité avec le format réglementaire, mais surtout vers l'exhaustivité ! En effet, les usages innovants des données des marchés publics n'émergeront que si les données sont exhaustives.

## Conversion du CSV breton vers le format JSON réglementaire (avec XSLT)

Les données des marchés publics peuvent être publiées dans deux formats : XML et JSON. Je n'ai encore jamais fait de conversion de CSV vers JSON, je commence donc par celle-ci.

Dans tous les cas, j'ai décidé de m'appuyer sur le langage de conversion [XSLT 3.0](https://www.w3.org/TR/xslt-30/#what-is-xslt), qui permet de faire des conversions de données entre XML et JSON et qui a l'avantage d'être standardisé par le W3C.

**Mise à jour du 3/10/17** : *XSLT 3.0 était testé ici à titre expérimental, mais s'avère être très lent. Il sera peut être utile pour des conversion XML <-> JSON ou XML <-> XML. Pour une méthode performante, voir [mon expérience avec jq ci-dessous](#avec-jq)*

Le fichier de départ est `marches-publics-collectivites-bretonnes.csv` (5749 entrées), téléchargeable sur [le site My Breizh Open Data](https://breizh-sba.opendatasoft.com/explore/dataset/marches-publics-collectivites-bretonnes/export/).


Voici les étapes de la conversion avec des liens vers les scripts utilisés et des échantillons des données intermédiaires :

| #   | Source                                                                                                                           | Résultat                                                                                                                                                                                                                            | Outil                                                                                                                                                                                        | Commande                                                                                                      | Temps de conversion |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------- |
| 1   | [CSV breton (5749 entrées))](https://breizh-sba.opendatasoft.com/explore/dataset/marches-publics-collectivites-bretonnes/table/) | CSV breton avec noms de procédures valides                                                                                                                                                                                          | [Script Shell (utilitaire sed)](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-1-mapping-des-procedures-sh)                                                       | `./mapping-des-procédures.sh marches-publics-collectivites-bretonnes.csv`                                     | 2,9 secondes        |
| 2   | CSV breton avec noms de procédures valides                                                                                       | [XML tabulaire](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2-tabulaire-xml)                                                                                                                          | [Script Python](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2-conversion-xml-tabulaire-py)                                                                     | `./conversion-xml-tabulaire.py marches-publics-collectivites-bretonnes.csv > tabulaire.xml`                   | 0,4 seconde         |
| 3   | [XML tabulaire](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2-tabulaire-xml)                       | [Format JSON réglementaire](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2d-format-reglementaire-json) ([format paquet](https://github.com/etalab/format-commande-publique/tree/master/exemples/json)) | [Saxon HE 9.8 pour Java](http://saxonica.com/download/java.xml), [Script XSLT 3.0](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2b-xml-tabulaire-vers-json-xsl) | `java -jar saxon9he.jar -s:tabulaire.xml tabulaire-vers-représentation-json.xsl -o:format-réglementaire.json` | 3,6 secondes        |

### Étape 1

Les noms de procédures utilisés par les collectivités varient beaucoup, et ne coorespondent pas aux valeurs attendues par le format réglementaire. Pour rappel, pour comparer des données, il est indispensable qu'elles se basent sur le même référentiel.

En l'occurrence, voici les noms de procédure trouvés dans les données bretonnes (j'ai omis les données fantaisistes) :

- Achat direct
- Appel d'offre ouvert
- Appel d'offre ouvert (art.33)
- Appel d'offre restreint
- Appel d'offres
- Concours
- Contrat de mandat
- MAPA - art 28
- MAPA - art 30 - au dessus des seuils
- MAPA - art 30 - en dessous des seuils
- Marché négocié
- Négocié avec pub (art.35I)
- Proc. adaptée/allégée (art.28et30)
- Procédure adaptée
- Procédure adaptée (MAPA)
- Procédure négociée après pub.
- Procédure négociée sans pub.
- UGAP

Qui doivent donc être "mappées", mises en correspondance avec les valeurs acceptées par le format réglementaire :

- Procédure adaptée
- Appel d'offres ouvert
- Appel d'offres restreint
- Procédure concurrentielle avec négociation
- Procédure négociée avec mise en concurrence préalable
- Marché négocié sans publicité ni mise en concurrence préalable
- Dialogue compétitif

Pour ce faire, [mon script](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-1-mapping-des-procedures-sh) remplace les valeurs non valides par leur équivalent issu du format réglementaire. Le Service de la commande publique et de la politique d’achat (SCPPA) du Conseil Régional de Bretagne m'a gentiment fourni [un tableau de correspondance](https://files.maudry.fr/f.php?h=2YZlHAGn&d=1).

Les valeurs n'ayant pas d'équivalent sont maintenues et elles seront ignorées au moment de la transformation vers JSON (étape #3).

### Étape 2

Pour pouvoir travailler avec XSLT, la première condition est d'avoir des données source au format XML. Nos données sont actuellement au format CSV, il faut donc y remédier.

Pour cette étape, il existe une pléthore d'options. J'ai opté pour une solution à la fois simple et multi-plateforme, [un script Python](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-2-conversion-xml-tabulaire-py).

Celui-ci parcours le fichier CSV ligne par ligne.

- Pour chaque ligne un élément `<row>` est créé
- Pour chaque cellule, un élément est créé avec pour nom le nom de la colonne, et avec pour valeur la valeur de la cellule.

Voici le résultat pour les deux premières lignes du CSV :

<script src="https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0.js?file=2-tabulaire.xml"></script>

### Étape 3

À présent que nous disposons d'un fichier XML, nous pouvons utiliser les possibilités offertes par XSLT 3.0 pour :

1. Convertir cet XML tabulaire dans une [représentation XML du format JSON réglementaire](https://www.w3.org/TR/xslt-30/#json-to-xml-mapping) (voir aussi [la commande qui permet de créer cette représentation](https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0#file-3a-commande-json-to-xml-sh))
2. Convertir la représentation XML en véritable JSON

Ces deux opérations sont menées à bien par la même feuille de style XSL, que voici :

<script src="https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0.js?file=3b-xml-tabulaire-vers-json.xsl "></script>

L'essentiel de ce code sert donc à créer la représentation XML du JSON réglementaire. Cette représentation est ensuite convertie en JSON à la ligne 21. Notons que le JSON produit est compact, sans aucun retour à la ligne.

Voici à quoi le JSON produit ressemble pour les deux premières lignes du CSV, après avoir été indenté :

<script src="https://gist.github.com/ColinMaudry/74464b8bc02e0e3786873dc1c7175cc0.js?file=3c-format-réglementaire.json"></script>

<a id="avec-jq"/>
## Conversion du CSV breton vers le format JSON réglementaire (avec jq)

Après avoir joué avec la sérialisation JSON de XSLT 3.0, je me devais de proposer une solution qui soit optimale, tant en terme de performance que de maintenance (configuration, utilisation d'un outil populaire). [jq](https://stedolan.github.io/jq/) remplit parfaitement ces critères.

Voici les étapes de la conversion avec des liens vers les scripts utilisés et des échantillons des données intermédiaires :

| #   | Source                                                                                                                           | Résultat                                                                                                                      | Outil | Commande                                                                                               | Temps de conversion     |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------ | ----------------------- |
| 1   | [CSV breton (5749 entrées))](https://breizh-sba.opendatasoft.com/explore/dataset/marches-publics-collectivites-bretonnes/table/) | [tabulaire.json](https://gist.github.com/ColinMaudry/78bd2fa840b3a10225d9f4ce61a2f678#file-1-tabulaire-json)                                                                                                                | [Script Python csv2json](https://github.com/nephridium/csv2json/blob/98956b1a888838520141aa71c672f680447ca3de/csv2json.py)      | `./csv2json.py -H 0 -S ";" marches-publics-collectivites-bretonnes.csv tabulaire.json`                 | 0,6 seconde             |
| 2   | [tabulaire.json](https://gist.github.com/ColinMaudry/78bd2fa840b3a10225d9f4ce61a2f678#file-1-tabulaire-json)                                                                                                                   | [Format JSON réglementaire](https://gist.github.com/ColinMaudry/78bd2fa840b3a10225d9f4ce61a2f678#file-2c-format-reglementaire-json) ([format paquet](https://github.com/etalab/format-commande-publique/tree/master/exemples/json)) |   [jq 1.5](https://stedolan.github.io/jq/), [filtre jq](https://gist.github.com/ColinMaudry/78bd2fa840b3a10225d9f4ce61a2f678#file-2a-jq-filter-jq)    | `jq --slurpfile procedures procedures.json -f jq-filter.jq tabulaire.json > format-réglementaire.json` | 0,3 seconde (oui oui !) |

### Étape 1

### Étape 2

## Validation du format JSON

Afin de vérifier la conformité des données produites avec le schéma des données essentielles, il faut procéder à une validation. La majorité des [outils de validation](http://json-schema.org/implementations.html#validators) au moyen de schémas JSON ne permettent pas de facilement valider des données avec un schéma "distant", qui ne se trouve pas sur le même système (dont l'adresse commence souvent par "http"). Quant aux [outils en ligne](http://json-schema.org/implementations.html#online), ils nous limitent dans la taille des données à valider.

J'étais donc heureux de trouver [json-schema-remote](https://github.com/entrecode/json-schema-remote) qui se spécialise dans la validation de données distantes avec schéma distant. Il peut fonctionner en ligne de commande, et est donc parfait pour une utilisation en local avec d'importants volumes de données à valider.

Pour "simuler" l'aspect distant des données, je les ajoute à mon serveur local : http://localhost/format-réglementaire.json, autrement, [ça ne semble pas marcher](https://github.com/entrecode/json-schema-remote/issues/1).

J'utilise le schéma ["paquet"](https://github.com/etalab/format-commande-publique/tree/master/exemples/json) car je valide un fichier contenant plusieurs marchés, et non un seul.

Voici la commande pour valider les données :

```
json-schema-remote http://localhost/format-réglementaire.json https://raw.githubusercontent.com/etalab/format-commande-publique/master/sch%C3%A9mas/json/paquet.json
```

Ce JSON n'est pas conforme au schéma en raisons des données manquantes dans les données sources (code CPV, date de publication des données, SIRET de l'acheteur, nature, forme du prix, etc.).

## Conclusion

Cette expérience décrit les étapes nécessaires pour convertir des données de marchés publics au format CSV vers le format JSON réglementaire, du nettoyage des données jusqu'à la validation du résulat.

Je souhaitais également montrer que le défi auquel font face les acheteurs publics français n'est pas la simple conversion des données vers le format réglementaire. En effet, pour les données bretonnes, il ne m'a fallu qu'un peu plus d'une journée pour arriver à des données propres et valides.

La difficulté consiste à faire en sorte que ces données existent, soient dématérialisées et qu'elles convergent vers un même point pour être converties.

N'hésitez pas à réagir dans les commentaires ci-dessous ou à me contacter directement par mail (colin@maudry.com) ou sur Twitter ([@col1m](https://twitter.com/col1m)).
