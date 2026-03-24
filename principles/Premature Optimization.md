# Principe : Premature Optimization

"L'optimisation prématurée est la racine de tous les maux" — Donald Knuth. L'idée : ne pas optimiser tant qu'on n'a pas mesuré un vrai problème de performance.

## Concept fondamental

L'optimisation prématurée survient quand on tente d'améliorer les performances avant d'avoir une compréhension claire des vrais problèmes de performance. On sacrifie la lisibilité et la maintenabilité du code pour des gains souvent négligeables.

La bonne approche :
1. Écrire du code clair et correct d'abord
2. Mesurer les performances (profiler, benchmarks)
3. Identifier les vrais points chauds
4. Optimiser uniquement ce qui pose problème

Ça ne veut pas dire qu'il faut écrire du code volontairement lent. Choisir le bon algorithme et la bonne structure de données dès le départ, c'est du bon sens, pas de l'optimisation prématurée. C'est quand on commence à sacrifier la lisibilité pour gratter des micro-secondes sans preuve de besoin que ça devient problématique.

## Exemple

Un développeur passe 2h à optimiser une boucle qui tourne en 3ms, alors que la page met 2 secondes à charger à cause d'une requête SQL non indexée. L'effort est mal placé.

Autre cas classique : réécrire une méthode avec des bitwise operations pour "gagner en performance" alors que le code devient illisible et que le gain est de 0.01ms sur une opération qui n'est appelée que 10 fois par minute.

## Avantages et inconvénients

**Avantages (de suivre ce principe) :**
- Le code reste lisible et maintenable
- On concentre ses efforts là où ça compte vraiment
- Les optimisations ciblées, basées sur des mesures, sont plus efficaces

**Inconvénients :**
- Peut être utilisé comme excuse pour ne jamais penser aux performances
- Certains choix architecturaux (structure de données, algorithme) sont coûteux à changer plus tard
- Il faut quand même avoir le réflexe de mesurer, ce qui demande des outils et du temps

## Sans ce principe

Sans suivre ce principe, on sacrifie la lisibilité pour des gains inexistants :

```java
// "Optimisé" — illisible
int result = (value << 1) + (value << 3); // multiplication par 10
int idx = hash & (capacity - 1);           // modulo quand capacity est une puissance de 2
boolean even = (n & 1) == 0;               // parité via bitwise

// Simple — clair
int result = value * 10;
int idx = hash % capacity;
boolean even = n % 2 == 0;
```

Le compilateur fait déjà ces optimisations. On a rendu le code illisible pour un gain de 0 nanoseconde. Pendant ce temps, la vraie cause de lenteur (une requête SQL sans index qui prend 3 secondes) n'a pas été traitée.

Mesurer d'abord, optimiser ensuite — et seulement là où ça compte.

## Liens avec les autres concepts

**Principes proches :**
- [Keep It Simple, Stupid (KISS)](Keep%20It%20Simple,%20Stupid%20(KISS).md) — l'optimisation prématurée viole KISS. On rend le code illisible pour des gains non mesurés. KISS rappelle : la clarté d'abord, l'optimisation ensuite.
- [You Aren't Gonna Need It (YAGNI)](You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md) — optimiser un code qui n'est pas un goulot d'étranglement, c'est du travail inutile. YAGNI dit : résous les problèmes qui existent, pas ceux que tu imagines.

**Pratiques liées :**
- [Performance Optimizing](../practices/Performance%20Optimizing.md) — complémentaires mais opposés dans le timing. L'optimisation prématurée dit **quand ne pas** optimiser (tant qu'on n'a pas mesuré). Performance Optimizing dit **comment** optimiser (profiler, cibler, mesurer à nouveau). Le premier est un principe, le second est la pratique qui l'accompagne.
- [Automated Testing](../practices/Automated%20Testing.md) — les tests de charge et les benchmarks sont l'outil qui permet de détecter les vrais problèmes de performance. Sans tests, on devine — et on devine mal.
- [Spiking](../practices/Spiking.md) — quand on hésite entre deux approches pour la performance, un spike permet de mesurer sans investir du temps de dev complet.
- [Code Reviewing](../practices/Code%20Reviewing.md) — un reviewer peut détecter une optimisation prématurée : "pourquoi cette complexité alors qu'un code simple ferait pareil ?"
