# Principe : High Cohesion

La cohésion, c'est le degré auquel les éléments d'un module travaillent ensemble vers un même objectif. Un module avec une haute cohésion fait une chose et la fait bien. Un module avec une faible cohésion fait un peu de tout — et c'est le bazar.

## Concept fondamental

Un module hautement cohérent contient des fonctionnalités qui sont étroitement liées et qui interagissent fréquemment entre elles. Les fonctionnalités non liées sont séparées dans d'autres modules. La cohésion élevée va souvent de pair avec un faible couplage : des modules bien découpés sont naturellement plus indépendants les uns des autres.

Il existe différents niveaux de cohésion, du plus faible au plus fort :
- **Cohésion fonctionnelle** — les éléments sont regroupés par tâche commune (le meilleur)
- **Cohésion séquentielle** — les éléments s'exécutent dans un ordre précis
- **Cohésion communicationnelle** — les éléments agissent sur les mêmes données
- **Cohésion temporelle** — les éléments sont regroupés car ils s'exécutent au même moment
- **Cohésion logique** — les éléments sont regroupés par relations logiques (le plus faible)

## Exemple

Un module de traitement de commandes devrait contenir :
- La validation des données de commande
- Le calcul du montant total
- La génération de la facture
- L'envoi de la confirmation

Tout ça est lié au même sujet : la commande. C'est cohérent.

Par contre, si ce même module gère aussi l'authentification des utilisateurs et l'envoi de newsletters, la cohésion est faible — ces responsabilités n'ont rien à voir entre elles.

En pratique, quand une classe a trop de méthodes qui ne se rapportent pas au même sujet, c'est un signe qu'il faut la découper.

## Avantages et inconvénients

**Avantages :**
- Facilite la compréhension du code et la localisation des fonctionnalités
- Réduit la complexité en limitant les interactions entre les composants
- Favorise la réutilisabilité et la modularité du code
- Les modifications ciblées sont plus faciles et moins risquées

**Inconvénients :**
- Peut mener à un trop grand nombre de petits modules si on pousse trop loin
- Parfois la frontière entre "lié" et "pas lié" n'est pas évidente
- Nécessite de repenser la structure quand les responsabilités évoluent

## Sans ce principe

Sans haute cohésion, on se retrouve avec des "god classes" qui font tout :

```java
class UserManager {
    public void createUser(String name) { /* ... */ }
    public void sendEmail(String to, String body) { /* ... */ }
    public byte[] generatePdfReport() { /* ... */ }
    public void backupDatabase() { /* ... */ }
    public String formatCurrency(double amount) { /* ... */ }
}
```

Quel rapport entre créer un utilisateur et sauvegarder la base de données ? Aucun. Cette classe a zéro cohésion. Modifier le format de la monnaie risque de casser la génération de PDF. Et impossible de réutiliser `formatCurrency()` sans embarquer le backup de base de données.

Avec une haute cohésion, chaque classe regroupe des fonctionnalités liées : `UserService`, `EmailService`, `ReportGenerator`, `BackupService`. Chacune est compréhensible, testable et réutilisable.

## Liens avec les autres concepts

**Principes proches :**
- [Single Responsibility](Single%20Responsibility.md) — très proches mais pas identiques. Le SRP dit "une seule raison de changer", la haute cohésion dit "les éléments d'un module doivent être liés entre eux". En pratique, respecter l'un mène naturellement à respecter l'autre. La différence : le SRP se concentre sur les **raisons de changer**, la cohésion sur le **degré de relation** entre les éléments.
- [Loose Coupling](Loose%20Coupling.md) — les deux vont de pair. Des modules hautement cohésifs sont naturellement faiblement couplés : quand chaque module a une responsabilité claire, il a moins de raisons de dépendre des autres.
- [Separation of Concerns](Separation%20of%20Concerns.md) — la séparation des préoccupations pousse vers la haute cohésion. Séparer les préoccupations = regrouper les choses liées ensemble et séparer les choses non liées.
- [Encapsulation](Encapsulation.md) — un module cohésif encapsule naturellement ses données et ses comportements. La cohésion et l'encapsulation se renforcent mutuellement.

**Point de vigilance :** haute cohésion ne veut pas dire "une méthode par classe". Un `OrderService` peut avoir `createOrder()`, `cancelOrder()`, `getOrderById()` — c'est cohésif car tout tourne autour des commandes. Le problème arrive quand il a aussi `sendEmail()` et `generatePDF()`.
