# Mémos — React · Un panier persistant et compréhensible

À relire pour expliquer les décisions ; les mémos ne remplacent pas les preuves de [l’évaluation](EVALUATION.md).

## État minimal, données dérivées

Le panier conserve des choix : produit et quantité. Les noms et prix viennent d’une lecture serveur ; total et compteur se calculent. Réduire l’état stocké évite de devoir maintenir plusieurs copies cohérentes. Cette règle n’interdit pas toute mise en cache : elle demande de nommer la provenance et la fraîcheur d’une donnée mise en cache.

## Reducer pur et effets

Une transition pure ne lance ni requête ni écriture navigateur. Elle peut être testée avec des entrées et sorties simples. La persistance et la relecture synchronisent le résultat avec des systèmes externes ; elles appartiennent à une autre couche. Une action porte une intention compréhensible, par exemple modifier la quantité d’un produit identifié, et ne décrit pas une manipulation du DOM.

## Une restauration est une entrée

Le navigateur peut contenir un document ancien, incomplet ou modifié. JSON.parse ne prouve que la syntaxe JSON ; la forme métier reste à vérifier. Reconstruire une structure autorisée limite les champs inattendus. Un type TypeScript ne résout pas cette frontière. Le code de restauration doit également tenir compte d’une lecture ou d’une écriture qui lève une exception.

## Estimation, disponibilité et décision

Un sous-total calculé depuis un prix récemment lu informe l’utilisateur. Il n’engage pas le serveur pour une opération future. La disponibilité peut évoluer après la lecture ; le contrôle décisif aura lieu pendant la réservation. Si l’API est indisponible, conserver l’intention locale aide à reprendre, mais ne justifie pas d’afficher « disponible » à partir d’un ancien objet.

## Relecture et pagination

Restaurer trois identifiants ne signifie pas qu’ils appartiennent tous à la première page. Une relecture par ID rend cette dépendance explicite. Charger seulement douze produits puis supprimer les lignes absentes confondrait absence de la page et absence du catalogue. Le même raisonnement vaut pour une recherche filtrée : ses résultats ne représentent pas tout le magasin.

## Accessibilité comme comportement

Un contrôle de quantité a besoin d’un nom, d’instructions et d’une erreur liée. Une action répétée sur plusieurs lignes a besoin de contexte. Lorsque retirer une ligne supprime le contrôle actif, une règle de focus évite de perdre la position. Ces choix se vérifient dans un scénario ; une simple présence de mots ARIA dans le code ne démontre pas leur qualité.
