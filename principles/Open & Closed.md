# Principe : Open/Closed

Le principe Open/Closed est un concept fondamental de conception logicielle qui promeut la conception de systèmes logiciels de manière à ce qu'ils soient ouverts à l'extension (Open) mais fermés à la modification (Closed). Ce principe encourage à concevoir des composants logiciels de manière à pouvoir ajouter de nouvelles fonctionnalités ou comportements sans modifier le code existant.

## Concept Fondamental :

Le principe Open/Closed repose sur l'idée que les entités logicielles telles que les classes, les modules ou les composants doivent être conçues de manière à ce qu'elles puissent être étendues pour prendre en charge de nouvelles fonctionnalités sans nécessiter de modifications dans le code existant. Cela favorise la robustesse, la stabilité et la réutilisabilité du code.

## Principes Clés :

1. **Extension sans Modification :** Les composants logiciels doivent être conçus de manière à permettre l'ajout de nouvelles fonctionnalités par extension, sans altérer le comportement existant.

2. **Utilisation de l'Abstraction :** Utiliser des abstractions (interfaces, classes abstraites) pour définir des contrats et permettre aux composants d'être étendus par substitution.

3. **Fermeture aux Modifications :** Éviter de modifier le code existant pour ajouter de nouvelles fonctionnalités, ce qui réduit le risque de régressions et de conflits.

## Exemple :

Imaginons un scénario où une équipe développe un système de paiement en ligne qui prend en charge différents modes de paiement tels que carte de crédit, PayPal, et virement bancaire. Pour appliquer le principe Open/Closed, l'équipe pourrait concevoir le système de paiement de manière à permettre l'ajout de nouveaux modes de paiement sans modifier le code existant.

Voici un exemple simplifié en Java :

```java
// Interface pour définir un mode de paiement
public interface PaymentMethod {
    void processPayment(double amount);
}

// Implémentation d'un mode de paiement par carte de crédit
public class CreditCardPayment implements PaymentMethod {
    public void processPayment(double amount) {
        System.out.println("Paiement de " + amount + " € effectué par carte de crédit.");
        // Logique de traitement du paiement par carte de crédit
    }
}

// Classe pour gérer le système de paiement
public class PaymentProcessor {
    public void processPayment(PaymentMethod paymentMethod, double amount) {
        paymentMethod.processPayment(amount);
    }
}

// Exemple d'utilisation du PaymentProcessor
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor = new PaymentProcessor();

        // Utilisation du paiement par carte de crédit
        PaymentMethod creditCardPayment = new CreditCardPayment();
        processor.processPayment(creditCardPayment, 100.0);

        // Ajout d'un nouveau mode de paiement (PayPal) sans modifier le code existant
        // (Open to extension, Closed to modification)
        PaymentMethod paypalPayment = new PaypalPayment();
        processor.processPayment(paypalPayment, 50.0);
    }
}
```

Dans cet exemple, le principe Open/Closed est appliqué en utilisant des interfaces (`PaymentMethod`) pour définir un contrat commun pour les différents modes de paiement. Le `PaymentProcessor` peut traiter n'importe quel mode de paiement en utilisant l'abstraction `PaymentMethod`, ce qui permet d'ajouter de nouveaux modes de paiement (comme PayPal) sans modifier le code du `PaymentProcessor`.

## Avantages du Principe Open/Closed :

- Réduction du risque de régressions : Les modifications sont limitées aux extensions, pas aux modifications directes du code existant.
- Amélioration de la réutilisabilité : Les composants peuvent être réutilisés et étendus plus facilement.
- Favorise une conception orientée vers les abstractions : Encourage l'utilisation de contrats (interfaces, classes abstraites) pour définir des comportements extensibles.

En conclusion, le principe Open/Closed est un principe important de la conception logicielle qui encourage une architecture flexible et extensible, en permettant l'ajout de nouvelles fonctionnalités sans compromettre l'intégrité du code existant.
