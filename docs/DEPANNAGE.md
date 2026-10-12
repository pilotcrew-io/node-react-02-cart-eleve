# Dépannage raisonné

Reproduire le symptôme sur un état connu, lire la réponse ou la trace, formuler une hypothèse puis effectuer un contrôle ciblé. Ne pas effacer tout le projet pour faire disparaître l’erreur.

| Symptôme | Hypothèse à vérifier | Contrôle et correction |
| --- | --- | --- |
| Total ancien après retrait | État dérivé stocké séparément | Recalculer depuis lignes et produits courants. |
| Produit absent après recharge | Lecture limitée à la première page | Relire par ID et distinguer 404 de l’absence de la page. |
| Quantité décimale acceptée | Conversion ou arrondi silencieux | Refuser les valeurs non entières à la frontière. |
| Boucle de restauration | Effet lisant et réécrivant sans distinction | Séparer initialisation validée, transitions et synchronisation. |
| Écran cassé après JSON corrompu | Parsing non protégé | Capturer l’erreur et restaurer une structure sûre avec explication. |
| Panier perdu lors d’une panne | Erreur réseau assimilée à panier vide | Conserver IDs/quantités et proposer une relecture. |
| Focus perdu après retrait | Contrôle actif supprimé sans règle | Appliquer et tester la destination choisie. |

Si le blocage concerne une installation manquante du Day 0, le noter séparément du résultat métier. Demander une aide ciblée après avoir inscrit la commande, le statut et le message utile dans le journal.
