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
