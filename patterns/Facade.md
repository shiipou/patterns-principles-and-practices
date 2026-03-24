# Design Pattern : Facade

La Facade est un pattern structurel qui fournit une interface simplifiée devant un ensemble de classes complexes. Au lieu de manipuler plein de sous-systèmes un par un, on passe par un point d'entrée unique qui orchestre tout.

## Concept fondamental

La façade ne remplace pas les sous-systèmes — elle les masque. Le code client n'a besoin de connaître qu'une seule classe avec une API simple, alors qu'en coulisses la façade coordonne plusieurs composants dans le bon ordre. Les sous-systèmes restent accessibles directement si besoin (contrairement à l'encapsulation stricte), mais dans la majorité des cas, la façade suffit.

C'est un pattern de simplification, pas d'abstraction : on ne change pas le comportement, on simplifie l'accès.

## Exemple

On a un site e-commerce avec trois sous-systèmes : gestion des stocks, facturation et expédition. Plutôt que de forcer le code client à appeler chaque sous-système dans le bon ordre, on crée une façade qui encapsule tout ça.

```java
class StockManager {
    public void checkStockAvailability(String product) {
        System.out.println("Vérification du stock pour : " + product);
    }
}

class BillingSystem {
    public void generateInvoice(String product, int quantity) {
        System.out.println("Facture générée pour " + quantity + "x " + product);
    }
}

class ShippingSystem {
    public void shipProduct(String product, String address) {
        System.out.println("Expédition de " + product + " vers " + address);
    }
}

// La façade : un seul appel pour tout gérer
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
        stockManager.checkStockAvailability(product);
        billingSystem.generateInvoice(product, quantity);
        shippingSystem.shipProduct(product, address);
    }
}

public class Main {
    public static void main(String[] args) {
        OnlineShoppingFacade shop = new OnlineShoppingFacade();
        shop.purchaseProduct("Smartphone", 1, "123 Rue Principale");
        shop.purchaseProduct("Laptop", 2, "456 Avenue du Parc");
    }
}
```

Le code client appelle juste `purchaseProduct()` et la façade s'occupe d'appeler les sous-systèmes dans le bon ordre. Si demain on ajoute un système de fidélité ou un anti-fraude, on le branche dans la façade sans toucher au code appelant.

## Avantages et inconvénients

**Avantages :**
- Réduit le couplage entre le code client et les sous-systèmes complexes
- Simplifie l'utilisation d'un système en offrant une API claire et concise
- Permet de modifier les sous-systèmes internes sans impacter les clients de la façade

**Inconvénients :**
- La façade peut devenir un "god object" si elle accumule trop de responsabilités
- Peut masquer la complexité réelle du système, ce qui complique le débogage
- N'empêche pas le code client d'accéder directement aux sous-systèmes s'il n'y a pas de contrôle d'accès

## Sans ce pattern

Sans Facade, le code client manipule tous les sous-systèmes directement :

```java
class OrderController {
    public void placeOrder(Cart cart, PaymentInfo payment, Address address) {
        // Vérifier le stock
        InventoryService inventory = new InventoryService();
        for (Item item : cart.getItems()) {
            if (!inventory.checkStock(item.getId(), item.getQuantity())) {
                throw new OutOfStockException(item);
            }
        }
        // Calculer le prix
        PricingService pricing = new PricingService();
        double total = pricing.calculateTotal(cart);
        total = pricing.applyDiscount(total, cart.getPromoCode());
        // Payer
        PaymentGateway gateway = new PaymentGateway();
        gateway.authorize(payment, total);
        gateway.capture(payment, total);
        // Expédier
        ShippingService shipping = new ShippingService();
        shipping.createLabel(address, cart.getWeight());
        // Notifier
        EmailService email = new EmailService();
        email.sendConfirmation(cart.getUserEmail(), cart);
    }
}
```

Ce même code est dupliqué dans `OrderController`, `MobileOrderController`, `ApiOrderController`… Si l'étape de paiement change, il faut modifier chaque controller. Et chaque controller doit connaître 5 services différents.

Avec une Facade, tout ça tient en une ligne : `shoppingFacade.placeOrder(cart, payment, address)`. Le controller ne connaît qu'un seul point d'entrée.
