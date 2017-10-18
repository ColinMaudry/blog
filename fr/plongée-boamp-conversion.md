

# Contexte

Le Bulletin Officiel des Annonces de Marchés Publics (BOAMP) est publié par la Direction de l'Information Légale et Administrative (DILA), rattachée aux services du Premier Ministre.

Le BOAMP recense une partie des annonces légales de marchés publics publiées par des acheteurs publics (commune, ministère, conseil régional, hôpital public, etc.). Le BOAMP recense princiaple deux types d'annonces :

- **les appels d'offre**, lorsqu'un acheteur public publie un appel aux entreprises privées pour concourir pour la réalisation d'un marché public
- **les avis d'attribution**, lorsqu'un acheteur public publie le résultat d'un appel d'offre, avec notamment le nom des entreprises retenues et le montant du marché public

La DILA publie [les données du BOAMP en Open Data](https://www.data.gouv.fr/fr/datasets/boamp/), au format XML. Ces données sont loin de décrire tous les marchés publics français, car ils ne sont pas tous annoncés sur le BOAMP. Le nombre de marchés recensés est cependant conséquent.

> **Note :** Je travaille actuellement avec la mission Etalab afin d'accompagner les différents acteurs de l'ouverture des données des marchés publics. L'expérimentation décrite dans cet article est principalement réalisée sur mon temps libre, bénévolement. Les tâches réalisées dans le cadre de ma prestation seront explicitement indiquées.


# Objectifs

Mon expérimentation vise, par ordre chronologique :

- à republier les données du BOAMP sous une forme plus facilement requêtable et exploitable
- à évaluer la part des marchés publics français recensés au BOAMP
- à faire des retours à la DILA pour l'amélioration du BOAMP
- à évaluer la possiblité de convertir les données du BOAMP au [format réglementaire des données essentielles de la commande publique](https://github.com/etalab/format-commande-publique)
- à évaluer la possibilité de convertir les données du BOAMP au [format OCDS](http://standard.open-contracting.org/latest/en/)
