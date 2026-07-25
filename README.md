# Réparation d'un Denon RCD-N8 : alimentation en veille défaillante

Journal de bord de la réparation d'un ampli Denon RCD-N8 qui refusait de sortir de son mode veille. [À COMPLÉTER : date de l'intervention]

## Le symptôme

L'ampli reste bloqué en veille : impossible de le démarrer, et pire, la led stand-by (rouge) elle-même ne s'allume pas. Ce dernier point oriente d'emblée le diagnostic vers l'alimentation, puisque même le circuit censé être actif en permanence (le stand-by) semble mort.

## Le diagnostic

Sur ce modèle, l'alimentation à découpage est en réalité composée de deux circuits distincts, chacun piloté par son propre contrôleur PWM :

- **IC821** : contrôleur du circuit d'alimentation principale.
- **IC871** : contrôleur du circuit stand-by, celui qui doit rester actif en permanence pour alimenter la led rouge et l'électronique de veille.

À l'ouverture, des traces de surchauffe sont visibles autour d'IC871 (référence ICE3BR1765J). Les mesures confirment le composant mort : infini sur toutes les broches, plus aucune continuité exploitable.

En creusant autour du contrôleur, le condensateur C874 (47µF/50V) est également suspect : sa capacité mesurée n'est plus que de 2µF, loin des 47µF attendus. Un condensateur de filtrage aussi dégradé explique en grande partie la surchauffe et la mort du contrôleur associé.

## La réparation — première tentative

Remplacement à l'identique :

- IC871 (ICE3BR1765J) neuf
- C874 (47µF/50V) neuf

Test de remise sous tension avec une ampoule 40W en série sur le secteur, par précaution.

L'ampli redémarre normalement. Cependant, après 20 minutes en veille, IC871 atteint 72°C. C'est dans les specs du composant, mais c'est chaud pour un usage en continu, et ça n'augure rien de bon pour la durée de vie à moyen terme.

## La réparation — solution retenue

Plutôt que de re-remplacer à l'identique en espérant un composant de meilleure qualité, le choix est de monter en gamme sur le contrôleur : **ICE3BR0665J** à la place de l'ICE3BR1765J d'origine, soit 74W de capacité contre 46W. Marge de fonctionnement bien plus confortable pour un usage en veille permanente.

Ce changement de contrôleur impose de revoir plusieurs composants périphériques pour que le nouveau circuit fonctionne dans de bonnes conditions :

- **C874** : remplacé (déjà changé en première tentative)
- **C875** (0,47µF/50V) : remplacé
- **C872** et **C821** : remplacés par des condensateurs polypropylène 10nF 5% 1000V
- **R871** : remplacée par 2 résistances de 33kΩ (2W, oxyde métallique) montées en série pour obtenir les 66kΩ requis par le nouveau contrôleur — la valeur d'origine (100kΩ) ne convient plus, et 66kΩ n'existe pas en valeur standard
- **C13** (en parallèle de la diode D871) : supprimé purement et simplement. Ce condensateur générait environ 10°C de chauffe supplémentaire et ne figure pas sur les autres schémas d'alimentation utilisant l'ICE3BR0665J — sa présence ici n'avait donc pas lieu d'être conservée.

Composants achetés chez [À COMPLÉTER : fournisseur], outils utilisés pour le désoudage : [À COMPLÉTER].

## Résultat

Après 70 minutes en veille, IC871 se stabilise autour de 42°C, contre 72°C en 20 minutes avec le remplacement à l'identique. L'ampli redémarre et fonctionne normalement.

## Conclusion

La leçon principale de cette réparation : remplacer un composant mort à l'identique ne suffit pas toujours, surtout quand la panne initiale s'explique par un sous-dimensionnement chronique. Ici, l'ICE3BR1765J d'origine tournait déjà proche de ses limites thermiques en usage normal, ce qui a probablement contribué à sa mort prématurée. Passer à un contrôleur mieux dimensionné (ICE3BR0665J), quitte à devoir adapter les composants périphériques en conséquence, s'est révélé être la vraie réparation durable.
