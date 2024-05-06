# Principe : High Cohesion

"High Cohesion" (cohésion élevée) est un principe de conception logicielle qui encourage à regrouper les fonctionnalités et les responsabilités liées étroitement au sein d'un même composant ou module. Ce concept vise à maximiser l'efficacité, la maintenabilité et la réutilisabilité du code en favorisant des composants bien définis et spécialisés dans une tâche spécifique.

## Concept Fondamental :

La cohésion élevée se réfère au degré auquel les éléments d'un module ou composant sont liés entre eux et travaillent ensemble pour atteindre un objectif commun. Un module avec une cohésion élevée contient des fonctionnalités qui sont étroitement liées et interagissent fréquemment entre elles, tandis que les fonctionnalités non liées sont séparées en d'autres modules.

## Principes Clés :

1. **Responsabilités Claires :** Chaque module ou composant doit avoir une seule et unique responsabilité bien définie.

2. **Faible Couplage :** La cohésion élevée est souvent associée à un faible couplage, ce qui signifie que les interactions entre les modules sont minimisées et bien définies.

3. **Modularité :** Les modules hautement cohérents sont plus modulaires, ce qui facilite la réutilisation et la maintenance du code.

### Niveaux de Cohésion :

Il existe plusieurs niveaux de cohésion, du plus faible au plus élevé :

- **Cohésion Fonctionnelle (Functional Cohesion) :** Les éléments d'un module sont regroupés en fonction de leur tâche ou fonctionnalité commune.

- **Cohésion Séquentielle (Sequential Cohesion) :** Les éléments d'un module sont exécutés dans un ordre spécifique, mais ne sont pas nécessairement liés par une fonctionnalité commune.

- **Cohésion Communicationnelle (Communicational Cohesion) :** Les éléments d'un module agissent sur les mêmes données ou partagent des informations.

- **Cohésion Temporelle (Temporal Cohesion) :** Les éléments d'un module sont regroupés car ils doivent être exécutés au même moment.

- **Cohésion Procédurale (Procedural Cohesion) :** Les éléments d'un module sont liés par une séquence de pas d'une tâche.

- **Cohésion Logique (Logical Cohesion) :** Les éléments d'un module sont regroupés par des relations logiques et algorithmiques.

## Exemple :

Imaginons un module de traitement des commandes dans un système de commerce électronique. Un module hautement cohérent pour le traitement des commandes pourrait inclure les fonctionnalités suivantes :

- Validation des données de la commande.
- Calcul du montant total de la commande.
- Génération des factures et des reçus.
- Envoi de notifications de confirmation au client.

En regroupant ces fonctionnalités liées au traitement des commandes dans un module distinct, on obtient une cohésion élevée qui favorise la réutilisabilité et la maintenabilité du code. Chaque modification ou ajout de fonctionnalité dans ce module est isolé et n'affecte pas les autres parties du système.

## Avantages de la Cohésion Élevée :

- Facilite la compréhension du code et la localisation des fonctionnalités.
- Réduit la complexité en limitant les interactions entre les composants.
- Favorise la réutilisabilité et la modularité du code.
- Améliore la maintenabilité en facilitant les modifications ciblées.

En conclusion, la cohésion élevée est un principe important de conception logicielle qui contribue à créer des modules et des composants spécialisés, bien définis et interconnectés. Cela conduit à des systèmes plus robustes, flexibles et faciles à maintenir sur le long terme.
