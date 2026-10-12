# Évaluation et preuves

Le barème porte sur ce qui est **démontré et expliqué**. Une suite non exécutée ou une capture absente n’est pas un succès implicite. Les choix internes peuvent différer du corrigé si le contrat et les critères sont respectés.

| Critère | Points / 20 | Preuve attendue |
| --- | --- | --- |
| Actions et invariants | 3 | Tableau des transitions et limites de lignes/quantités explicites. |
| Source d’état et composants | 4 | Transitions cohérentes et absence de total dupliqué. |
| Persistance validée | 4 | Document minimal, corruption et champs inattendus maîtrisés. |
| Relecture et estimation | 3 | Produits hors première page retrouvés et données périmées signalées. |
| Clavier et tests comportementaux | 3 | Ajout, modification et retrait reproduits sans souris. |
| Journal et explication des limites | 3 | Preuves de recharge/panne et périmètre multi-onglets décrit. |

## Questions d’explication

1. Pourquoi ne pas persister le total comme donnée de référence ?
2. Que prouve JSON.parse sur un panier restauré ?
3. Que faire si un produit du panier n’apparaît pas dans la page courante ?
4. Pourquoi conserver l’intention lors d’une erreur réseau ?
5. Quel comportement de focus attendez-vous après le retrait de la ligne active ?

## Format de preuve

Pour chaque contrôle : commit, environnement, commande ou action, résultat attendu, résultat observé, interprétation et lien vers le rapport/capture. Séparer backend, frontend, navigateur et éventuelle CI distante. Pour une mesure : conserver aussi protocole et valeurs brutes.

Un invariant critique non respecté reste non acquis même si les points de présentation sont obtenus. L’évaluation est formative ; elle n’atteste ni une mise en production, ni un niveau senior, ni la sécurité d’un service public.
