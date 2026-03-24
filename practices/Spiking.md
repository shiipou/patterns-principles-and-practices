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
