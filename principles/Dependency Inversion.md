# Principe : Dependency Inversion

Le Dependency Inversion dit que le code de haut niveau (la logique métier) ne doit pas dépendre du code de bas niveau (les détails d'implémentation). Les deux doivent dépendre d'abstractions. En pratique : on dépend d'interfaces, pas de classes concrètes.

## Concept fondamental

Le principe repose sur deux règles :
1. Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau — les deux doivent dépendre d'abstractions (interfaces)
2. Les abstractions ne doivent pas dépendre des détails — les détails doivent dépendre des abstractions

En pratique, ça se traduit par l'injection de dépendances : au lieu de créer ses dépendances en interne (`new ConcreteService()`), on les reçoit de l'extérieur via le constructeur ou un setter.

## Exemple

Sans inversion de dépendance, le `Client` est collé à une implémentation concrète :

```java
class Client {
    private ConcreteService service;

    public Client() {
        this.service = new ConcreteService(); // Dépendance directe
    }

    public void doSomething() {
        this.service.performOperation();
    }
}
```

Si on veut changer de service, il faut modifier `Client`. Pas top.

Avec inversion de dépendance, on passe par une interface :

```java
interface Service {
    void performOperation();
}

class Client {
    private Service service;

    public Client(Service service) {
        this.service = service; // Injection via le constructeur
    }

    public void doSomething() {
        this.service.performOperation();
    }
}

class ConcreteService implements Service {
    public void performOperation() {
        System.out.println("Opération effectuée.");
    }
}
```

Maintenant le `Client` ne connaît que l'interface `Service`. On peut lui injecter n'importe quelle implémentation — y compris un mock pour les tests.

## Avantages et inconvénients

**Avantages :**
- Réduit le couplage entre les modules de haut et bas niveau
- Facilite les tests unitaires (on peut injecter des mocks)
- Permet de changer d'implémentation sans modifier le code client
- Favorise une architecture modulaire et évolutive

**Inconvénients :**
- Ajoute de l'indirection : il faut suivre les interfaces pour comprendre le flux
- Nécessite un mécanisme d'injection (constructeur, framework DI) qui peut être complexe
- Peut être du sur-engineering si le module de bas niveau ne changera jamais

## Sans ce principe

Sans inversion de dépendance, les tests deviennent un cauchemar :

```java
class OrderService {
    private MySQLDatabase db = new MySQLDatabase("prod-url");
    private StripePayment payment = new StripePayment("real-api-key");

    public void placeOrder(Order order) {
        db.save(order);
        payment.charge(order.getTotal());
    }
}
```

Comment tester `placeOrder()` sans une vraie base MySQL et un vrai compte Stripe ? Impossible. Le code métier est collé aux implémentations concrètes.

Avec l'inversion de dépendance, `OrderService` dépend d'interfaces (`Database`, `PaymentGateway`). En test, on injecte des mocks. En prod, on injecte les vraies implémentations. Le code métier ne change pas.

## Liens avec les autres concepts

**Principes proches :**
- [Abstraction](Abstraction.md) — la Dependency Inversion **nécessite** des abstractions (interfaces) pour fonctionner. Sans interface `Service`, on ne peut pas inverser la dépendance. L'abstraction est le mécanisme, la DI est le principe qui dit de l'utiliser.
- [Loose Coupling](Loose%20Coupling.md) — la DI est le **principal levier** pour obtenir un couplage lâche. En dépendant d'interfaces plutôt que de classes concrètes, les modules deviennent interchangeables.
- [Open/Closed](Open%20&%20Closed.md) — grâce à la DI, on peut ajouter de nouvelles implémentations sans modifier le code client. Les deux principes se renforcent mutuellement.
- [Single Responsibility](Single%20Responsibility.md) — la DI encourage à séparer les responsabilités : la création des objets est séparée de leur utilisation.

**Patterns qui appliquent ce principe :**
- [Strategy](../patterns/Strategy.md) — le contexte dépend de l'interface `Strategy`, pas de l'implémentation concrète.
- [Observer](../patterns/Observer.md) — le sujet dépend de l'interface `Observer`, pas des observateurs concrets.
- [Factory Method](../patterns/Factory%20Method.md) / [Abstract Factory](../patterns/Abstract%20Factory.md) — les factories permettent de créer des objets sans coupler le code client aux classes concrètes.
- [Adapter](../patterns/Adapter.md) — l'adaptateur implémente l'interface attendue et découple le client de la librairie concrète.

**Pratiques liées :**
- [Automated Testing](../practices/Automated%20Testing.md) — la DI rend le code **testable**. Sans elle, impossible de mocker les dépendances. C'est souvent le premier bénéfice concret que les développeurs ressentent.

**Tension avec le [Singleton](../patterns/Singleton.md) :** le Singleton crée un accès global (`getInstance()`), ce qui est l'opposé de l'injection de dépendances. Privilégier l'injection par constructeur au Singleton.
