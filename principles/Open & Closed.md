# Principe : Open/Closed

Un module doit être **ouvert à l'extension** mais **fermé à la modification**. Autrement dit, on doit pouvoir ajouter de nouveaux comportements sans toucher au code existant.

## Concept fondamental

Le principe repose sur l'utilisation d'abstractions (interfaces, classes abstraites) pour définir des contrats. Les composants sont conçus de manière à ce qu'on puisse les étendre par substitution (ajouter une nouvelle implémentation) sans altérer le comportement existant.

Le but : réduire le risque de régressions. Si on ne modifie pas le code qui marche, on ne casse rien. Les évolutions se font par ajout, pas par modification.

## Exemple

Un système de paiement qui supporte la carte de crédit. Demain on veut ajouter PayPal.

```java
interface PaymentMethod {
    void processPayment(double amount);
}

class CreditCardPayment implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Paiement de " + amount + " € par carte.");
    }
}

class PaymentProcessor {
    public void processPayment(PaymentMethod method, double amount) {
        method.processPayment(amount);
    }
}
```

Pour ajouter PayPal, on crée une nouvelle classe `PaypalPayment implements PaymentMethod`. Le `PaymentProcessor` n'a pas besoin d'être modifié — il fonctionne déjà avec n'importe quelle implémentation de `PaymentMethod`.

```java
class PaypalPayment implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Paiement de " + amount + " € via PayPal.");
    }
}
```

C'est ça le Open/Closed : le code existant reste intact, on étend le système par ajout.

## Avantages et inconvénients

**Avantages :**
- Réduit le risque de régressions : on n'altère pas le code qui fonctionne
- Améliore la réutilisabilité : les composants peuvent être étendus facilement
- Favorise une conception orientée vers les abstractions

**Inconvénients :**
- Nécessite de prévoir les points d'extension dès la conception initiale
- Peut mener à une sur-abstraction si on essaie de rendre *tout* extensible
- Parfois, modifier le code existant est plus simple et plus clair qu'ajouter une couche d'abstraction

## Sans ce principe

Sans Open/Closed, chaque évolution oblige à modifier le code existant :

```java
class PaymentProcessor {
    public void process(String type, double amount) {
        if (type.equals("credit_card")) {
            // traitement carte
        } else if (type.equals("paypal")) {
            // traitement PayPal
        } else if (type.equals("bitcoin")) {
            // ajouté la semaine dernière
        } else if (type.equals("apple_pay")) {
            // ajouté aujourd'hui
        }
    }
}
```

Chaque nouveau moyen de paiement modifie cette méthode. On touche du code qui marchait déjà, avec un risque de régression à chaque ajout. Et cette méthode grossit indéfiniment.

Avec le Open/Closed, on ajoute une nouvelle classe `ApplePayPayment implements PaymentMethod` sans toucher à `PaymentProcessor`. Le code existant reste intact.

## Liens avec les autres concepts

**Principes proches :**
- [Dependency Inversion](Dependency%20Inversion.md) — les deux se renforcent mutuellement. La DI dit "dépends d'abstractions", le Open/Closed dit "utilise ces abstractions pour étendre sans modifier". Sans inversion de dépendance, le Open/Closed est quasi impossible à appliquer.
- [Abstraction](Abstraction.md) — les abstractions (interfaces) sont le **mécanisme** qui rend le Open/Closed possible. L'interface `PaymentMethod` est le point d'extension.
- [Single Responsibility](Single%20Responsibility.md) — chaque implémentation (chaque nouveau moyen de paiement) a sa propre classe avec une seule responsabilité.
- [Loose Coupling](Loose%20Coupling.md) — un module ouvert à l'extension mais fermé à la modification est naturellement faiblement couplé aux implémentations concrètes.

**Tensions avec d'autres principes :**
- [KISS](Keep%20It%20Simple,%20Stupid%20(KISS).md) / [YAGNI](You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md) — rendre tout extensible "au cas où" ajoute de la complexité. Le Open/Closed doit être appliqué aux **vrais** points de variation, pas partout.

**Patterns qui appliquent ce principe :**
- [Strategy](../patterns/Strategy.md) — on ajoute une nouvelle stratégie sans modifier le contexte.
- [Decorator](../patterns/Decorator.md) — on ajoute des comportements sans modifier les classes existantes.
- [Observer](../patterns/Observer.md) — on ajoute des observateurs sans modifier le sujet.
- [Factory Method](../patterns/Factory%20Method.md) / [Abstract Factory](../patterns/Abstract%20Factory.md) — on ajoute de nouveaux types/familles sans modifier le code client.
