# Practices : Spiking

Un spike, c'est une exploration technique rapide et ciblée. Quand l'équipe fait face à une inconnue — nouvelle techno, approche non testée, faisabilité incertaine — on prend un temps limité pour expérimenter et lever le doute.

## Concept fondamental

On ne cherche pas à produire du code propre ou livrable. On cherche à répondre à une question : "Est-ce que ça marche ? Est-ce que c'est viable ? Quelles sont les limites ?" Un spike produit de la connaissance, pas du code de production. Le code du spike est généralement jeté — c'est normal, c'est son but.

Le spike est toujours limité dans le temps (quelques heures à une journée max). À la fin, l'équipe sait si l'approche est viable, combien de temps ça va prendre, et quels sont les risques.

## Exemple

Avant d'intégrer un nouveau moteur de recherche dans l'appli, on crée un petit projet à part pour tester l'API, mesurer les performances, et voir si ça correspond au besoin. En quelques heures, on a la réponse.

Quand faire un spike :
- Quand on ne sait pas estimer une tâche parce que l'inconnue technique est trop grande
- Quand on hésite entre plusieurs approches et qu'il faut tester pour trancher
- Quand on veut valider qu'une librairie ou un service tiers fait bien ce qu'on attend

## Avantages et inconvénients

**Avantages :**
- Réduit l'incertitude technique avant de s'engager sur une approche
- Permet de mieux estimer les tâches futures
- Rapide et peu coûteux comparé à un développement complet qui part dans la mauvaise direction

**Inconvénients :**
- Le code produit n'est pas réutilisable (c'est du jetable)
- Peut être utilisé comme excuse pour explorer sans contrainte de temps
- Si mal cadré, le spike dérive et ne répond pas à la question initiale

## Sans cette pratique

Sans spike, l'équipe estime une tâche à 3 jours sur la base de "on pense que ça devrait marcher". Au bout de 5 jours, on découvre que la librairie choisie ne supporte pas le cas d'usage, que l'API tierce a des limites non documentées, et qu'il faut tout recommencer avec une autre approche.

Un spike de quelques heures aurait répondu à la question avant de s'engager. L'équipe aurait su que l'approche était viable (ou pas), combien de temps ça prendrait vraiment, et quels étaient les risques.

Le spike évite de transformer une incertitude technique en surprises coûteuses.

## Liens avec les autres concepts

**Pratiques proches :**
- [Solution Sketching](Solution%20Sketching.md) — complémentaires. Le sketching explore **visuellement** ("quelle architecture ?"), le spike explore **techniquement** ("est-ce que cette lib marche ?"). Le sketching précède souvent le spike : on dessine l'approche, puis on code un prototype pour la valider.
- [Pair Programming](Pair%20Programming.md) — explorer à deux est souvent plus efficace : un développeur teste, l'autre réfléchit et suggère des pistes alternatives.
- [Automated Testing](Automated%20Testing.md) — un spike peut inclure des benchmarks ou des tests de faisabilité. Mais attention : le code du spike est jetable, les tests aussi.
- [Performance Optimizing](Performance%20Optimizing.md) — un spike est idéal pour comparer les performances de deux approches avant de choisir.

**Principes liés :**
- [YAGNI](../principles/You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md) — le spike est l'allié de YAGNI : au lieu de coder une feature "au cas où", on fait un spike pour valider si le besoin est réel et faisable **avant** de s'engager.
- [Premature Optimization](../principles/Premature%20Optimization.md) — plutôt que d'optimiser à l'aveugle, un spike permet de **mesurer** avant de choisir une approche.
- [KISS](../principles/Keep%20It%20Simple,%20Stupid%20(KISS).md) — un spike aide à trouver la solution la plus simple qui marche, en testant rapidement plusieurs approches.

**Point clé :** le code du spike est **jetable**. Ce n'est pas du code de production, et ce n'est pas grave. Le spike produit de la **connaissance**, pas du code. La valeur est dans la réponse à la question, pas dans le prototype.
