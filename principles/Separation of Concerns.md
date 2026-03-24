# Principe : Separation of Concerns

Chaque partie du code devrait s'occuper d'un seul sujet. L'affichage ne devrait pas connaître la logique métier. La logique métier ne devrait pas savoir comment les données sont stockées. Chaque "préoccupation" vit dans son propre coin.

## Concept fondamental

Le principe repose sur l'idée de découpler les différentes responsabilités d'un système pour que chaque composant puisse se concentrer sur un aspect unique. On réduit les dépendances entre les modules pour favoriser le découplage et la flexibilité, et on s'assure que chaque module a une responsabilité claire, sans chevauchement de fonctionnalités.

Ce découpage n'est pas réservé aux grosses architectures. Même dans un petit projet, séparer la logique dans des fonctions ou des fichiers dédiés plutôt que tout mélanger dans un seul fichier, c'est déjà appliquer la séparation des préoccupations.

## Exemple

Dans une appli web, on sépare généralement en trois couches :
- **Présentation** — l'interface utilisateur, l'affichage, les interactions
- **Logique métier** — les règles du domaine, les calculs, les validations
- **Accès aux données** — les requêtes en base, le stockage, la persistance

Si la façon dont on affiche un prix change, seule la couche présentation bouge. Si les règles de calcul de TVA changent, seule la couche métier est concernée. Si on migre de PostgreSQL à MongoDB, seule la couche données est impactée.

## Avantages et inconvénients

**Avantages :**
- Facilite la compréhension du code en isolant les responsabilités
- Les modules peuvent être réutilisés dans différents contextes
- Les changements dans une préoccupation n'affectent pas les autres parties du système
- Chaque partie peut être développée, testée et modifiée indépendamment

**Inconvénients :**
- Ajoute de la structure et des fichiers, ce qui peut sembler lourd sur de petits projets
- Peut mener à un découpage excessif si on sépare des choses qui sont naturellement liées
- La communication entre couches nécessite des interfaces claires

## Sans ce principe

Sans séparation des préoccupations, tout est mélangé dans la même méthode :

```java
public String handleRequest(HttpRequest request) {
    // Logique métier
    double price = getProductPrice(request.getParam("id"));
    double tax = price * 0.20;
    double total = price + tax;

    // Accès base de données
    Connection conn = DriverManager.getConnection("jdbc:mysql://...");
    PreparedStatement stmt = conn.prepareStatement("INSERT INTO orders ...");
    stmt.executeUpdate();

    // Génération HTML
    return "<html><body><h1>Total: " + total + " €</h1></body></html>";
}
```

SQL, HTML et logique métier dans la même méthode. Changer l'affichage risque de casser le calcul. Migrer de MySQL à PostgreSQL oblige à toucher du code qui gère aussi le HTML. Et impossible de tester le calcul de TVA sans une connexion à la base.

Avec la séparation des préoccupations, chaque couche (présentation, métier, données) vit dans ses propres classes. On peut modifier l'une sans impacter les autres.

## Liens avec les autres concepts

**Principes proches :**
- [Single Responsibility](Single%20Responsibility.md) — très proches mais à des échelles différentes. Le SRP s'applique au niveau de la **classe** ("une seule raison de changer"), la Separation of Concerns s'applique au niveau du **système** (séparer présentation, métier, données). Le SRP est la version micro de la Separation of Concerns.
- [High Cohesion](High%20Cohesion.md) — séparer les préoccupations crée des modules hautement cohésifs. Chaque couche regroupe des fonctionnalités liées.
- [Loose Coupling](Loose%20Coupling.md) — des préoccupations bien séparées sont faiblement couplées. La couche métier ne connaît pas les détails de la couche de données.
- [Encapsulation](Encapsulation.md) — chaque couche encapsule ses détails internes et n'expose qu'une interface à la couche au-dessus.

**Patterns qui appliquent ce principe :**
- [Facade](../patterns/Facade.md) — la Facade sépare le code client de la complexité d'un sous-système. Le client s'occupe du "quoi", la Facade du "comment".
- [Observer](../patterns/Observer.md) — sépare la détection d'un changement (le sujet) de la réaction à ce changement (les observateurs).
- [Template Method](../patterns/Template%20Method.md) — sépare la structure de l'algorithme (dans la classe parente) des détails d'implémentation (dans les sous-classes).
- [Decorator](../patterns/Decorator.md) — sépare les comportements additionnels (log, retry) du comportement de base.

**Pratiques liées :**
- [Solution Sketching](../practices/Solution%20Sketching.md) — dessiner l'architecture avant de coder aide à identifier les bonnes frontières entre préoccupations.
