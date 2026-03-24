# Practices : Retrospective

La rétro, c'est le moment où l'équipe prend du recul à la fin d'un sprint ou d'un projet pour se demander : qu'est-ce qui a bien marché ? Qu'est-ce qui a foiré ? Qu'est-ce qu'on change pour la prochaine fois ?

## Concept fondamental

La rétrospective est le mécanisme d'amélioration continue de l'équipe. L'idée est de réfléchir collectivement sur ce qui s'est passé, d'identifier les problèmes, et de décider d'actions concrètes pour les résoudre. Sans rétro, on répète les mêmes erreurs sprint après sprint.

Ce qui fait une bonne rétro :
- Un climat de confiance où chacun peut parler sans crainte
- Des actions concrètes et réalistes, pas juste des constats
- Un suivi réel d'un sprint à l'autre
- Un facilitateur qui s'assure que tout le monde s'exprime

## Exemple

Le déroulement classique :
1. Chacun partage ses retours — ce qui a été positif, ce qui a posé problème
2. L'équipe identifie les sujets les plus importants
3. On décide d'actions concrètes avec des responsables
4. Au sprint suivant, on vérifie si les actions ont été mises en place

Quelques formats populaires :
- **Start, Stop, Continue** — qu'est-ce qu'on commence à faire, qu'est-ce qu'on arrête, qu'est-ce qu'on continue
- **Glad, Sad, Mad** — ce qui nous rend contents, tristes ou frustrés
- **4L** — Liked, Learned, Lacked, Longed for

Exemple concret : à la fin d'un sprint, l'équipe constate que les reviews de code prennent trop de temps. Action décidée : limiter les PRs à 300 lignes max et assigner un reviewer dès la création de la PR.

## Avantages et inconvénients

**Avantages :**
- Favorise une culture d'amélioration continue
- Renforce la collaboration et la responsabilité collective
- Permet d'identifier rapidement les problèmes et de prendre des mesures correctives
- Fournit un canal pour un feedback constructif et ouvert

**Inconvénients :**
- Peut devenir un rituel vide si les actions ne sont jamais suivies
- Certaines personnes n'osent pas s'exprimer en groupe
- Prend du temps et peut être perçu comme "encore une réunion"
- Risque de tourner en rond sur les mêmes sujets sans résolution

## Sans cette pratique

Sans rétrospective, l'équipe répète les mêmes erreurs sprint après sprint. Le déploiement du vendredi a cassé la prod — encore. Les PRs restent bloquées 3 jours en attente de review — encore. Les specs arrivent incomplètes — encore.

Personne ne prend le temps de se poser la question : "Qu'est-ce qu'on pourrait changer ?" Les frustrations s'accumulent en silence, les problèmes deviennent "normaux", et l'équipe stagne.

La rétro est le mécanisme qui transforme les erreurs en améliorations. Sans elle, il n'y a pas de boucle de feedback.

## Liens avec les autres concepts

**Pratiques proches :**
- Toutes les autres pratiques — la rétro est une **méta-pratique**. C'est le moment où on évalue si les autres pratiques fonctionnent :
  - "Les [reviews](Code%20Reviewing.md) sont trop lentes" → action : limiter les PRs à 300 lignes
  - "On n'a pas assez de [tests](Automated%20Testing.md)" → action : imposer une couverture minimale
  - "Le [pairing](Pair%20Programming.md) est épuisant" → action : alterner avec du travail solo
  - "On passe du temps sur des features inutiles" → action : appliquer [YAGNI](../principles/You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md)

- [Code Reviewing](Code%20Reviewing.md) — si les reviews posent problème (trop lentes, superficielles, conflictuelles), c'est un sujet de rétro.
- [Pair Programming](Pair%20Programming.md) — la rétro est le moment d'ajuster les pratiques de pairing.
- [Solution Sketching](Solution%20Sketching.md) — la rétro peut révéler qu'on fonce trop vite dans le code sans dessiner d'abord.

**Principes liés :**
- La rétro est le mécanisme qui détecte les violations de principes au niveau de l'équipe. "On répète toujours le même code" → [DRY](../principles/Don't%20Repeat%20Yourself%20(DRY).md). "Nos classes sont trop grosses" → [SRP](../principles/Single%20Responsibility.md). "On a codé 3 features inutiles" → [YAGNI](../principles/You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md).

**Rôle unique :** la rétro est la seule pratique qui s'améliore **elle-même**. Si la rétro ne fonctionne pas, c'est un sujet… de rétro.
