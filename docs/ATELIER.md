# Tutoriel 02 — React · Un panier persistant et compréhensible

Le temps indiqué suppose [les prérequis](PREREQUIS.md) terminés. Le [contrat commun](PREREQUIS.md) est la référence pour les champs, routes et limites. Consigner les résultats au fil de l’eau dans [le journal](../travail/REPONSES.md).

## 1. Définir les actions du panier — 20 min

Vérifiez la provenance du code importé depuis l’atelier 01 et ouvrez le journal du nouveau fork. Écrivez les actions disponibles : ajouter, changer une quantité, retirer une ligne, vider explicitement et restaurer. Pour chaque action, indiquez un état avant, un état après et un cas refusé. Ce tableau guidera les composants et les tests ; il précède leur implémentation.

Fixez les invariants : au plus dix produits distincts, chaque quantité entière de un à cinq, aucun identifiant dupliqué. Ajouter le même produit modifie sa ligne ; une sixième unité ne doit pas apparaître silencieusement. Retirer une ligne se fait par une action claire plutôt qu’en saisissant une quantité ambiguë. Choisissez des textes qui expliquent la limite lorsqu’elle est atteinte.

Dessinez le panier vide, le panier rempli, la relecture en cours et la ligne dont le produit n’est plus disponible. Le total affiché est une estimation issue des derniers prix serveur lus. Il n’est pas une réservation et n’envoie encore aucun POST. L’utilisateur doit voir cette limite avant de décider de poursuivre à l’atelier suivant.

## 2. Centraliser les transitions — 45 min

Définissez une structure minimale : des couples productId et quantity. Centralisez les changements dans un reducer pur ou une fonction de transition équivalente. Une même entrée et une même action doivent produire le même résultat, sans lecture réseau et sans écriture localStorage dans cette fonction. Faites la différence entre une règle métier du panier et une préférence d’affichage.

Écrivez les transitions une par une, avec des tableaux de petits cas. Une action invalide peut être refusée et produire un message contrôlé ; choisissez une stratégie cohérente et documentez-la. Ne corrigez pas silencieusement 1.5 en 1 ni la chaîne « 2 » en entier si elle vient du stockage inconnu. Les saisies de formulaires sont des chaînes : leur conversion contrôlée appartient à la frontière du formulaire, avant l’action métier.

Calculez le nombre d’unités et le total estimé à partir des lignes et des produits relus. Ne gardez pas ces agrégats dans plusieurs useState indépendants. Expliquez quel bug apparaîtrait si retirer une ligne mettait à jour le tableau mais oubliait de modifier le total. Utilisez les identifiants produits comme clés de liste ; la position d’une ligne n’est pas son identité.

Avant de brancher le rendu, écrivez des tests de transitions pour panier vide, ajout répété, limite de cinq et retrait. Vérifiez que l’état précédent n’a pas été muté. Cette étape permet d’isoler une erreur de calcul d’un problème de clic ou de libellé.

## 3. Construire les contrôles accessibles — 65 min

Ajoutez à chaque fiche un bouton qui nomme le produit : plusieurs boutons « Ajouter » non contextualisés rendent le parcours difficile à expliquer et à tester. Présentez ensuite un accès au panier, son nombre de lignes et un compteur d’unités avec une formulation stable. Le message doit décrire le résultat de l’action, pas déplacer le focus à chaque ajout.

Dans le panier, donnez à chaque contrôle de quantité un libellé qui inclut le nom du produit. Gardez une action Retirer distincte. Les limites 1 et 5 sont visibles dans les instructions et contrôlées dans le code. Lorsqu’une action est impossible, indiquez pourquoi ; une couleur seule ne donne pas cette information. Le prix unitaire, la quantité et le sous-total doivent se lire sans inférence.

Déterminez le comportement du focus après retrait de la ligne qui le contient. Il peut rejoindre une action voisine ou le titre du panier vide, selon une règle documentée et testée. Évitez un retour arbitraire au début de toute la page. Préservez un accès à la liste des produits et un état vide qui explique comment continuer.

Reliez les composants à une seule source de transitions. Les composants enfants reçoivent les informations et les actions utiles ; ils ne réécrivent pas leur propre copie du panier global. Si un contexte simplifie le partage, expliquez ce qu’il remplace. Il n’est pas nécessaire d’ajouter une bibliothèque de gestion d’état pour dix lignes.

## 4. Persister une intention et relire les produits — 55 min

Créez une frontière de stockage dédiée. La clé pédagogique est `pilotcrew.retail.cart.v1`. N’y sauvegardez que l’identité de la version et les couples produit/quantité nécessaires ; aucun prix, stock, total ou objet complet de réponse ne doit devenir la source métier. Protégez lecture, parsing et écriture contre les exceptions. Un stockage bloqué doit permettre de poursuivre dans la session, avec une indication honnête sur la persistance.

Traitez les données restaurées comme unknown. Contrôlez forme, version, nombre de lignes, unicité des identifiants et quantités. Reconstruisez un nouvel objet à partir des champs autorisés. Une propriété surprise ne doit pas se propager par un spread d’objet non vérifié. Si le document est inutilisable, affichez un message compréhensible et repartez d’un état sûr, sans présenter cet incident comme un panier validé.

Relisez les produits nécessaires sur GET /api/products/{id}, ou en réutilisant une lecture de catalogue complète dont la couverture est prouvée. La page de douze produits ne contient pas forcément une ligne ajoutée sur une autre page. Tant que cette relecture manque, n’affichez pas un total ancien comme actuel et ne prétendez pas qu’une ligne est disponible. En cas d’erreur transitoire, conservez l’intention et proposez de réessayer.

Si une ancienne valeur d’affichage est disponible en mémoire, vous pouvez signaler un changement de prix ou de stock. Après un rechargement propre, la persistance minimale ne conserve pas les anciens prix : annoncez simplement que le montant a été recalculé avec le catalogue relu, sans inventer une comparaison. Un produit définitivement absent reçoit un état distinct permettant de le retirer.

## 5. Tester la restauration et les incidents — 60 min

Exécutez les tests de transitions, puis testez trois parcours React : ajout et retrait au clavier, rechargement avec restauration valide, stockage corrompu avec message utile. Dans chaque test, videz uniquement les clés du laboratoire et remettez les mocks à zéro. Les tests ne doivent pas dépendre du panier laissé par le cas précédent.

```bash
npm test
npm run build
```

Dans un navigateur local, ajoutez deux produits, changez une quantité puis rechargez. Inspectez la valeur stockée : expliquez pourquoi elle ne contient pas de prix. Arrêtez ensuite l’API avant un nouveau rechargement. Le panier ne doit pas disparaître au seul motif que la lecture réseau a échoué ; il indique que les données actuelles sont indisponibles. Relancez l’API et utilisez le réessai.

Sur cette origine locale uniquement, remplacez manuellement le document du panier par un JSON mal formé puis par des lignes dupliquées. Vérifiez la restauration sûre et l’absence d’erreur non contrôlée. Essayez aussi d’ajouter une propriété priceCents fabriquée : elle ne doit pas influencer le total. Conservez les observations réelles dans le journal, puis remettez un panier normal pour la démonstration.

Faites lire l’interface à 200 % et parcourez les contrôles sans souris. La stabilité du focus après retrait est un critère de comportement, pas une décoration. Une recherche par rôle dans le test peut signaler un nom ambigu ; elle ne remplace pas entièrement le parcours humain.

## 6. Expliquer les limites et remettre — 25 min

Montrez le code qui décide des transitions et celui qui dialogue avec localStorage. Votre binôme doit pouvoir indiquer lequel peut être testé sans DOM et lequel peut échouer à cause du navigateur. Demandez-lui de retrouver une ligne d’une page différente après restauration ; cette vérification détecte une dépendance cachée à la page courante.

Remettez les transitions, les contrôles, la restauration validée, les tests et les preuves. Expliquez la portée du choix : localStorage appartient à une origine et peut persister entre les visites, mais ce n’est ni un compte utilisateur ni une synchronisation entre appareils. Les conflits entre plusieurs onglets ne sont pas résolus par le socle ; ils deviennent un sujet d’extension documenté. Préparez le commit que le prochain fork reprendra.
