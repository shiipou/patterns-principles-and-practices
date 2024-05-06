# Design Pattern : Facade

Le design pattern Facade est un pattern structurel qui fournit une interface unifiée simplifiée pour un ensemble complexe de classes. Il permet de fournir un point d'entrée unique pour interagir avec un système complexe, masquant ainsi sa complexité interne.

**Exemple de Code :**

Imaginons un scénario où nous avons un système complexe de différents sous-systèmes (par exemple, gestion des stocks, facturation, expédition) et nous voulons fournir une interface simplifiée (une façade) pour les clients afin d'interagir avec ces sous-systèmes.

Voici comment nous pourrions utiliser le design pattern Facade pour résoudre ce problème :

```java
// Sous-système complexe : Gestion des stocks
class StockManager {
    public void checkStockAvailability(String product) {
        System.out.println("Vérification de la disponibilité du produit : " + product);
        // Logique de vérification de la disponibilité du produit
    }
}

// Sous-système complexe : Facturation
class BillingSystem {
    public void generateInvoice(String product, int quantity) {
        System.out.println("Génération de la facture pour " + quantity + " " + product);
        // Logique de génération de la facture
    }
}

// Sous-système complexe : Expédition
class ShippingSystem {
    public void shipProduct(String product, String address) {
        System.out.println("Expédition du produit " + product + " à l'adresse : " + address);
        // Logique d'expédition du produit
    }
}

// Façade : Interface simplifiée pour interagir avec les sous-systèmes
class OnlineShoppingFacade {
    private StockManager stockManager;
    private BillingSystem billingSystem;
    private ShippingSystem shippingSystem;

    public OnlineShoppingFacade() {
        this.stockManager = new StockManager();
        this.billingSystem = new BillingSystem();
        this.shippingSystem = new ShippingSystem();
    }

    public void purchaseProduct(String product, int quantity, String address) {
        // Vérifier la disponibilité du produit
        stockManager.checkStockAvailability(product);

        // Générer la facture
        billingSystem.generateInvoice(product, quantity);

        // Expédier le produit
        shippingSystem.shipProduct(product, address);
    }
}

// Exemple d'utilisation du pattern Facade
public class Main {
    public static void main(String[] args) {
        // Utilisation de la façade pour effectuer un achat en ligne
        OnlineShoppingFacade shoppingFacade = new OnlineShoppingFacade();
        shoppingFacade.purchaseProduct("Smartphone", 1, "123 Street, City");

        System.out.println(); // Saute une ligne

        // Autre exemple d'utilisation
        shoppingFacade.purchaseProduct("Laptop", 2, "456 Avenue, Town");
    }
}
```

Dans cet exemple, le design pattern Facade est utilisé pour encapsuler un système complexe de gestion des stocks, facturation et expédition dans une interface simplifiée (`OnlineShoppingFacade`). Les clients peuvent utiliser la façade (`purchaseProduct`) pour interagir avec ces sous-systèmes sans avoir à connaître les détails de leur implémentation.

Le pattern Facade permet de réduire la dépendance et la complexité en fournissant une couche d'abstraction simple et unifiée pour interagir avec des systèmes complexes.
