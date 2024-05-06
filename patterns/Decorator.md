# Design Pattern : Decorator

Le design pattern Decorator est un pattern structurel qui permet d'ajouter dynamiquement des fonctionnalités supplémentaires à un objet en utilisant une approche de composition plutôt que d'héritage. Il permet d'ajouter des comportements à des objets existants de manière flexible et transparente.

**Exemple de Code :**

Imaginons un scénario où nous avons une classe `Coffee` représentant une boisson de base, et nous voulons ajouter des décorateurs pour ajouter des extras tels que le lait, le sucre, ou la crème.

Voici comment nous pourrions utiliser le design pattern Decorator pour résoudre ce problème :

```java
// Interface pour représenter une boisson (composant de base)
interface Coffee {
    double getCost(); // Coût de la boisson
    String getDescription(); // Description de la boisson
}

// Implémentation concrète d'une boisson : Café simple
class SimpleCoffee implements Coffee {
    public double getCost() {
        return 2.0; // Coût d'un café simple
    }

    public String getDescription() {
        return "Café"; // Description d'un café simple
    }
}

// Classe abstraite pour les décorateurs
abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee decoratedCoffee) {
        this.decoratedCoffee = decoratedCoffee;
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }
}

// Décorateur concret : Ajoute du lait à la boisson
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    public double getCost() {
        return super.getCost() + 0.5; // Coût supplémentaire pour le lait
    }

    public String getDescription() {
        return super.getDescription() + ", Lait"; // Description avec ajout du lait
    }
}

// Décorateur concret : Ajoute du sucre à la boisson
class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    public double getCost() {
        return super.getCost() + 0.2; // Coût supplémentaire pour le sucre
    }

    public String getDescription() {
        return super.getDescription() + ", Sucre"; // Description avec ajout du sucre
    }
}

// Exemple d'utilisation du pattern Decorator
public class Main {
    public static void main(String[] args) {
        // Commande d'un café simple
        Coffee coffee = new SimpleCoffee();
        System.out.println("Commande : " + coffee.getDescription() + ", Coût : $" + coffee.getCost());

        // Ajout de lait au café
        coffee = new MilkDecorator(coffee);
        System.out.println("Commande : " + coffee.getDescription() + ", Coût : $" + coffee.getCost());

        // Ajout de sucre au café
        coffee = new SugarDecorator(coffee);
        System.out.println("Commande : " + coffee.getDescription() + ", Coût : $" + coffee.getCost());
    }
}
```

Dans cet exemple, le design pattern Decorator est utilisé pour ajouter dynamiquement des fonctionnalités (lait, sucre) à un objet `Coffee` de base (`SimpleCoffee`) sans modifier sa classe. Les classes `MilkDecorator` et `SugarDecorator` agissent comme des décorateurs qui enveloppent la boisson de base (`SimpleCoffee`) et ajoutent leurs propres fonctionnalités (coût et description supplémentaires).

Le pattern Decorator permet d'étendre les fonctionnalités d'un objet de manière flexible et modulaire, en évitant la multiplication des sous-classes pour chaque combinaison de fonctionnalités.
