# Practices : Pair Programming

Deux développeurs, un écran, un clavier. L'un code (le "pilote"), l'autre réfléchit et guide (le "navigateur"). On alterne les rôles régulièrement, toutes les 20-30 minutes.

## Concept fondamental

Le pair programming repose sur l'idée que deux cerveaux valent mieux qu'un. Le pilote écrit le code, le navigateur prend du recul, anticipe les problèmes et réfléchit à la direction générale. Cette dynamique produit du code de meilleure qualité avec moins de bugs.

Les formats principaux :
- **Driver-Navigator** — le classique. Le pilote tape, le navigateur prend du recul et réfléchit à la direction.
- **Ping-Pong** — un développeur écrit un test, l'autre écrit le code pour le faire passer. Puis on inverse. Ça marche très bien avec du TDD.

## Exemple

Quand utiliser le pair programming :
- Sur un problème complexe où deux cerveaux valent mieux qu'un
- Pour former un junior — le senior navigue, le junior pilote
- Sur du code critique qui ne doit pas avoir de bugs
- Pour découvrir une nouvelle partie du codebase avec quelqu'un qui la connaît

Comment bien le faire :
- Parler en continu : verbaliser ce qu'on fait et pourquoi
- Alterner les rôles pour que les deux restent engagés
- Rester focalisé sur la tâche — pas de slack, pas de mails
- Accepter qu'on ne code pas "deux fois plus vite", mais qu'on produit du code de meilleure qualité avec moins de bugs

## Avantages et inconvénients

**Avantages :**
- Meilleure qualité du code : deux paires d'yeux détectent plus d'erreurs
- Transfert de connaissances naturel entre les développeurs
- Réduit le "bus factor" : plusieurs personnes connaissent chaque partie du code
- Force à verbaliser sa pensée, ce qui clarifie les idées

**Inconvénients :**
- Plus coûteux en temps de développeur (deux personnes, une tâche)
- Peut être épuisant sur de longues périodes
- Ne fonctionne pas bien si les deux développeurs n'ont pas la bonne dynamique
- Pas adapté à toutes les tâches (tâches simples et répétitives par ex.)

## Sans cette pratique

Sans pair programming, un développeur travaille seul sur un problème complexe. Il part dans une direction pendant 3 jours, se rend compte que l'approche est mauvaise, et recommence. Personne n'a vu le problème venir parce que personne ne regardait.

Résultat classique :
- Un junior passe une journée sur un bug qu'un senior aurait résolu en 10 minutes
- Un développeur est le seul à connaître une partie du code — le jour où il part, personne ne peut le maintenir
- Des bugs subtils passent en production parce qu'une seule personne a pensé au problème

Le pair programming ne rend pas "deux fois plus rapide", mais le code produit a moins de bugs, l'équipe partage les connaissances, et les mauvaises approches sont détectées avant d'y avoir investi trop de temps.

## Liens avec les autres concepts

**Pratiques proches :**
- [Code Reviewing](Code%20Reviewing.md) — le pair programming est une review **en temps réel**. La review classique détecte les problèmes après le code, le pairing les détecte **pendant**. Les deux sont complémentaires : le pairing n'annule pas le besoin de review (un troisième œil est toujours utile).
- [Automated Testing](Automated%20Testing.md) — le format Ping-Pong est du TDD à deux : un développeur écrit le test, l'autre écrit le code pour le faire passer. Ça force à écrire des tests et produit un code bien testé.
- [Spiking](Spiking.md) — explorer une inconnue technique à deux est souvent plus efficace : un développeur teste, l'autre réfléchit et suggère des pistes.
- [Solution Sketching](Solution%20Sketching.md) — le pairing commence souvent par un croquis à deux sur un tableau blanc avant de toucher au code.

**Pratiques liées :**
- [Retrospective](Retrospective.md) — la rétro est le bon moment pour ajuster les pratiques de pairing (fréquence, durée, formation des paires).
