# Design Pattern : Template Method

Le design pattern Template Method est un pattern comportemental qui définit le squelette d'un algorithme dans une méthode, en laissant certaines étapes aux sous-classes. Cela permet aux sous-classes de redéfinir certaines étapes de l'algorithme sans changer sa structure globale.

**Exemple de Code :**

Imaginons un scénario où nous avons une classe abstraite `Recipe` représentant une recette de cuisine avec des étapes standardisées, mais permettant aux sous-classes de personnaliser certaines étapes spécifiques.

Voici comment nous pourrions utiliser le design pattern Template Method pour résoudre ce problème :

```java
// Classe abstraite représentant une recette
abstract class Recipe {
    // Méthode Template qui définit le processus de préparation de la recette
    public void prepareRecipe() {
        boilWater(); // Faire bouillir de l'eau
        brew(); // Infuser ou préparer l'ingrédient principal
        pourInCup(); // Verser dans une tasse
        addCondiments(); // Ajouter des condiments ou des garnitures
    }

    // Méthode abstraite à implémenter par les sous-classes
    abstract void brew();

    // Méthode abstraite à implémenter par les sous-classes
    abstract void addCondiments();

    // Méthode concrète pour faire bouillir de l'eau
    void boilWater() {
        System.out.println("Faire bouillir de l'eau");
    }

    // Méthode concrète pour verser dans une tasse
    void pourInCup() {
        System.out.println("Verser dans une tasse");
    }
}

// Implémentation concrète d'une recette : Café
class CoffeeRecipe extends Recipe {
    void brew() {
        System.out.println("Infuser du café moulu");
    }

    void addCondiments() {
        System.out.println("Ajouter du lait et du sucre selon les préférences");
    }
}

// Implémentation concrète d'une recette : Thé
class TeaRecipe extends Recipe {
    void brew() {
        System.out.println("Infuser du thé dans de l'eau chaude");
    }

    void addCondiments() {
        System.out.println("Ajouter du citron ou du miel selon les préférences");
    }
}

// Exemple d'utilisation du pattern Template Method
public class Main {
    public static void main(String[] args) {
        // Préparation d'une recette de café
        Recipe coffeeRecipe = new CoffeeRecipe();
        System.out.println("Recette de Café :");
        coffeeRecipe.prepareRecipe();

        System.out.println();

        // Préparation d'une recette de thé
        Recipe teaRecipe = new TeaRecipe();
        System.out.println("Recette de Thé :");
        teaRecipe.prepareRecipe();
    }
}
```

Dans cet exemple, le design pattern Template Method est utilisé pour définir le processus standard de préparation d'une recette (`prepareRecipe()`) dans la classe abstraite `Recipe`. Les étapes spécifiques comme l'infusion (`brew()`) et l'ajout de condiments (`addCondiments()`) sont laissées à implémenter par les sous-classes (`CoffeeRecipe` et `TeaRecipe`).

Le pattern Template Method permet de définir un squelette d'algorithme dans une classe de manière à ce que les étapes spécifiques puissent être redéfinies par les sous-classes sans changer la structure globale de l'algorithme.
