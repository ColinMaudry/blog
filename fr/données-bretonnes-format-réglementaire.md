# Conversion des données des marchés publics bretons au format réglementaire

La Région Bretagne a fait appel à mes services de conseil et de développement pour les accompagner dans la publication des données des marchés publics bretons. La première étape consistait à créer un script qui convertirait [les données déjà publiées](https://breizh-sba.opendatasoft.com/explore/dataset/marches-publics-collectivites-bretonnes/table/) vers [le format réglementaire](https://www.data.gouv.fr/fr/datasets/referentiel-de-donnees-marches-publics/).

## Mise en correspondance des champs ("mapping")


| Champ CSV breton               | Champ format réglementaire     | Commentaire                                                                   |
| ------------------------------ | ------------------------------ | ----------------------------------------------------------------------------- |
| Nmarché                        | id                             |                                                                               |
| SIRETMandataire                | acheteur.id                    |                                                                               |
| LibelleAcheteur                | acheteur.nom                   |                                                                               |
| Nature                         | nature                         |                                                                               |
| objet                          | objet                          | Limité à 256 caractère                                                        |
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
| SIRETContractant               | titulaires.id                  |                                                                               |
| DenominationSociale            | titulaires.denominationSociale |                                                                               |
| Role                           |                                | Les sous-traitants devront être écartés.                                      |
| CodePostal, Dpt ID, Commune... |                                | Ces données sur les titulaires pourront être récupérées via l'API SIREN.      |
|                                | datePublicationDonnees         |                                                                               |
|                                | formePrix                      | Ferme / Ferme et actualisable / Révisable                                     |
|                                | modifications                  |                                                                               |


Au final, seuls trois champs du format réglementaire restent insatisfaits. Lorsque les collectivités auront rempli toutes les colonnes du format CSV breton, elles auront fait une très grande part du chemin vers la conformité avec le format réglementaire, mais surtout vers l'exhaustivité ! En effet, les usages innovants des données des marchés publics n'émergeront que si les données sont exhaustives.
