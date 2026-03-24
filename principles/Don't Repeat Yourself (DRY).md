# Principe : Don't Repeat Yourself (DRY)

Si un bout de logique ou une information apparaît à plusieurs endroits dans le code, il y a un problème. Le jour où ça change, il faut penser à tout mettre à jour — et on en oublie toujours un. DRY, c'est simplement : chaque information ne doit avoir qu'une seule source de vérité.

## Concept fondamental

Le principe DRY dit qu'une pièce d'information ou de logique ne doit être représentée qu'une seule fois dans le système. Au lieu de copier-coller du code, on le factorise dans des fonctions, des modules ou des composants réutilisables.

Ça ne concerne pas que les constantes. DRY s'applique à :
- La logique dupliquée → extraire une fonction
- Les structures répétées → créer une classe ou un composant réutilisable
- La documentation → ne pas maintenir la même info à deux endroits

## Exemple

Le taux de TVA est utilisé à plusieurs endroits. Au lieu de mettre `0.20` en dur partout :

```java
public class PricingService {
    private static final double VAT_RATE = 0.20;

    public double calculateTotalPrice(double price) {
        return price * (1 + VAT_RATE);
    }
}

// Utilisation
PricingService pricing = new PricingService();
double totalPrice = pricing.calculateTotalPrice(100);
System.out.println("Prix TTC : " + totalPrice); // 120.0
```

Si la TVA passe à 21%, on change une seule ligne. Sans ça, il faudrait chercher tous les `0.20` dans le code et prier pour n'en avoir oublié aucun.

## Avantages et inconvénients

**Avantages :**
- Réduit la duplication et favorise la réutilisation du code
- Les modifications ne sont à faire qu'à un seul endroit, ce qui simplifie la maintenance
- Le code est plus clair et plus lisible

**Inconvénients :**
- Parfois on force une abstraction entre deux bouts de code qui se ressemblent *par hasard* mais n'ont pas la même raison de changer
- Extraire trop agressivement peut créer des dépendances artificielles entre des modules qui devraient être indépendants
- Il faut trouver le bon équilibre : toute duplication n'est pas forcément mauvaise

## Sans ce principe

Sans DRY, la même logique est dupliquée dans plusieurs endroits :

```java
class OrderService {
    public double calculateTotal(List<Item> items) {
        double total = 0;
        for (Item item : items) { total += item.getPrice() * item.getQuantity(); }
        total *= 1.20; // TVA
        if (total > 100) total *= 0.95; // réduction 5%
        return total;
    }
}

class InvoiceService {
    public double getInvoiceAmount(List<Item> items) {
        double total = 0;
        for (Item item : items) { total += item.getPrice() * item.getQuantity(); }
        total *= 1.20; // TVA
        if (total > 100) total *= 0.95; // réduction 5%
        return total;
    }
}
```

Le jour où la TVA passe à 21%, il faut retrouver **tous** les endroits où `1.20` apparaît. Et on en oublie toujours un. Le bug ne sera découvert que quand un client recevra une facture incohérente.

Avec DRY, un seul `PricingService.calculateTotal()` est appelé partout. Un changement = un seul endroit à modifier.
