# Practices : Solution Sketching

Avant de coder, on dessine. Le solution sketching, c'est poser ses idées visuellement — sur papier, tableau blanc ou outil de wireframing — pour explorer des pistes et valider une direction avant d'investir du temps de développement.

## Concept fondamental

L'idée est simple : ça coûte 5 minutes de dessiner un schéma, et 3 jours de recoder une mauvaise approche. Le solution sketching permet de générer rapidement plusieurs concepts, de récolter du feedback très tôt, et d'aligner l'équipe sur une même vision avant de se lancer.

Le but n'est pas de produire un livrable joli, c'est de penser à voix haute avec les mains. On itère vite : esquisser, montrer, ajuster, recommencer.

## Exemple

On peut esquisser :
- Un wireframe d'interface pour se mettre d'accord sur la structure d'une page
- Un schéma d'architecture pour visualiser les composants et leurs interactions
- Un diagramme de séquence pour clarifier un flux complexe
- Un simple croquis sur papier pour comparer deux approches
- Un storyboard pour illustrer des scénarios d'utilisation

Pas besoin d'un outil sophistiqué. Un tableau blanc et un marqueur font l'affaire.

Exemple concret : une équipe travaille sur une appli mobile de gestion de tâches. Plutôt que de foncer dans le code, ils dessinent des wireframes de l'interface, les montrent aux utilisateurs potentiels, et récupèrent du feedback avant d'avoir écrit une seule ligne.

## Avantages et inconvénients

**Avantages :**
- Rapide : on génère des idées sans investir du temps de développement
- Favorise la collaboration et la communication dans l'équipe
- Réduit les risques en identifiant les problèmes tôt dans le processus
- Permet de valider une direction avec les utilisateurs avant de coder

**Inconvénients :**
- Peut donner un faux sentiment de validation (un croquis n'est pas un prototype fonctionnel)
- Risque de passer trop de temps à dessiner au lieu de coder
- Tous les types de problèmes ne se prêtent pas à une représentation visuelle

## Sans cette pratique

Sans solution sketching, l'équipe fonce directement dans le code. Deux développeurs partent chacun de leur côté avec une vision différente de l'architecture. Au bout de 3 jours, les deux approches sont incompatibles et il faut jeter du travail.

Autre cas classique : on développe une fonctionnalité pendant deux semaines, on la montre au product owner, et la réponse est "ce n'est pas du tout ce que j'avais en tête". Un croquis de 5 minutes sur un tableau blanc aurait évité deux semaines de travail perdu.

Le sketching coûte quelques minutes. Le code mal orienté coûte des jours.

## Liens avec les autres concepts

**Pratiques proches :**
- [Spiking](Spiking.md) — complémentaires. Le sketching explore visuellement ("à quoi ça ressemble ?"), le spike explore techniquement ("est-ce que ça marche ?"). Souvent, un sketch mène à un spike : on dessine l'architecture, puis on code un prototype pour valider.
- [Pair Programming](Pair%20Programming.md) — le pairing commence souvent par un sketch à deux sur un tableau blanc. Le sketching est le préambule naturel du pairing sur un sujet complexe.
- [Retrospective](Retrospective.md) — la rétro peut révéler qu'on fonce trop souvent dans le code sans prendre le temps de dessiner d'abord.

**Principes liés :**
- [YAGNI](../principles/You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md) — le sketching aide à identifier ce qui est **vraiment nécessaire** avant de coder. Un croquis révèle vite les fonctionnalités superflues.
- [KISS](../principles/Keep%20It%20Simple,%20Stupid%20(KISS).md) — comparer deux croquis d'architecture aide à choisir la solution la plus simple.
- [Separation of Concerns](../principles/Separation%20of%20Concerns.md) — un schéma d'architecture rend la séparation des préoccupations **visible**. On voit immédiatement si une couche en fait trop.

**Patterns :**
- Le sketching est le meilleur moment pour décider quels [design patterns](../patterns/) utiliser. Dessiner les composants et leurs interactions révèle naturellement le besoin d'une [Facade](../patterns/Facade.md), d'un [Observer](../patterns/Observer.md) ou d'une [Factory](../patterns/Factory%20Method.md).
