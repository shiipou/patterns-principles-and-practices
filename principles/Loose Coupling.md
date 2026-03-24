# Principe : Loose Coupling

Le faible couplage, c'est faire en sorte que les composants d'un système soient le plus indépendants possible les uns des autres. Chaque module communique avec les autres via des interfaces bien définies, sans connaître leurs détails internes.

## Concept fondamental

Le faible couplage se réfère à la manière dont les composants logiciels interagissent. Quand les modules sont fortement couplés, modifier l'un casse l'autre. Avec un faible couplage, on peut changer l'implémentation d'un module sans toucher au reste — tant que l'interface est respectée.

Pour y arriver :
- Passer par des interfaces ou des classes abstraites plutôt que des classes concrètes
- Utiliser l'injection de dépendances pour fournir les dépendances de l'extérieur
- Communiquer entre modules via des événements ou des messages plutôt que des appels directs
- Utiliser des patterns comme Observer ou Strategy pour déléguer des fonctionnalités à des composants externes

Le faible couplage va souvent de pair avec la haute cohésion : des modules bien découpés, chacun responsable d'un sujet précis, qui communiquent entre eux par contrat.

## Exemple

Dans un site e-commerce, le module de commandes ne devrait pas appeler directement les méthodes internes du module de paiement. Il passe par une interface `PaymentService`. Si demain on change de prestataire de paiement, le module de commandes ne bouge pas — on remplace juste l'implémentation derrière l'interface.

Même logique pour le module de gestion des utilisateurs : il expose une interface pour créer, modifier et supprimer des utilisateurs, mais il ne connaît pas les détails d'implémentation des autres modules.

## Avantages et inconvénients

**Avantages :**
- Les composants peuvent être développés, testés et déployés indépendamment
- Les composants faiblement couplés sont plus faciles à réutiliser dans d'autres projets
- Les modifications dans un composant n'affectent pas les autres
- Réduit les effets de bord et la propagation des changements

**Inconvénients :**
- Ajoute de l'indirection (interfaces, événements) qui peut rendre le code plus difficile à suivre
- Peut complexifier l'architecture si on découple des choses qui n'en ont pas besoin
- Nécessite une bonne conception des interfaces — un mauvais contrat couplé à une interface ne résout rien

## Sans ce principe

Sans faible couplage, chaque classe instancie directement ses dépendances :

```java
class OrderService {
    public void placeOrder(Order order) {
        MySQLDatabase db = new MySQLDatabase();
        db.save(order);

        SmtpEmailSender sender = new SmtpEmailSender("smtp.gmail.com");
        sender.send(order.getEmail(), "Commande confirmée");

        StripePayment stripe = new StripePayment();
        stripe.charge(order.getTotal());
    }
}
```

Changer Gmail pour SendGrid ? Modifier `OrderService`. Changer Stripe pour PayPal ? Modifier `OrderService`. Tester sans envoyer de vrais emails ? Impossible. Cette classe est couplée à 3 implémentations concrètes.

Avec un faible couplage, `OrderService` dépend d'interfaces (`Database`, `EmailSender`, `PaymentGateway`). On peut changer l'implémentation, mocker pour les tests, ou remplacer un fournisseur sans toucher à la logique métier.
