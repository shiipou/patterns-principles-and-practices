# Design Pattern : Template Method

Le Template Method définit le squelette d'un algorithme dans une classe parente, en laissant les sous-classes redéfinir certaines étapes. La structure globale est figée, mais les détails sont personnalisables.

## Concept fondamental

La classe parente contient une méthode "template" (souvent `final`) qui définit l'ordre des étapes de l'algorithme. Certaines étapes sont implémentées directement (comportement commun), d'autres sont déclarées abstraites et laissées aux sous-classes. Le `final` garantit que personne ne peut changer l'ordre des étapes — seul le contenu de chaque étape varie.

C'est l'inverse de la Strategy : ici c'est la classe parente qui contrôle le flux, les sous-classes ne font que remplir les trous.

## Exemple

On a une recette de boisson chaude. Les étapes sont toujours les mêmes (bouillir l'eau, infuser, verser, ajouter les extras), mais le contenu de chaque étape varie selon qu'on prépare un café ou un thé.

```java
abstract class HotBeverage {
    // Le template : l'ordre est fixe
    public final void prepare() {
        boilWater();
        brew();
        pourInCup();
        addExtras();
    }

    private void boilWater() { System.out.println("Faire bouillir l'eau"); }
    private void pourInCup() { System.out.println("Verser dans la tasse"); }

    // Ces étapes sont laissées aux sous-classes
    abstract void brew();
    abstract void addExtras();
}

class Coffee extends HotBeverage {
    void brew() { System.out.println("Infuser le café moulu"); }
    void addExtras() { System.out.println("Ajouter du lait et du sucre"); }
}

class Tea extends HotBeverage {
    void brew() { System.out.println("Infuser le thé"); }
    void addExtras() { System.out.println("Ajouter du citron"); }
}

public class Main {
    public static void main(String[] args) {
        HotBeverage coffee = new Coffee();
        coffee.prepare();

        System.out.println("---");

        HotBeverage tea = new Tea();
        tea.prepare();
    }
}
```

La méthode `prepare()` est `final` — personne ne peut changer l'ordre des étapes. Par contre, `brew()` et `addExtras()` sont abstraites : c'est la sous-classe qui décide quoi mettre dedans. Le squelette reste identique, seuls les détails changent.

## Avantages et inconvénients

**Avantages :**
- L'algorithme est défini une seule fois dans la classe parente — pas de duplication
- Le `final` protège la structure de l'algorithme contre les modifications accidentelles
- Les sous-classes n'ont qu'à implémenter les étapes spécifiques, pas tout l'algorithme

**Inconvénients :**
- La hiérarchie d'héritage peut devenir rigide si l'algorithme évolue beaucoup
- Les sous-classes sont fortement couplées à la classe parente
- Si le nombre d'étapes personnalisables est grand, l'interface de la classe abstraite devient lourde

## Sans ce pattern

Sans Template Method, les classes similaires dupliquent toute la structure :

```java
class CoffeeMaker {
    public void prepare() {
        boilWater();           // commun
        brewCoffee();          // spécifique
        pourInCup();           // commun
        addSugar();            // spécifique
    }
}

class TeaMaker {
    public void prepare() {
        boilWater();           // commun (dupliqué)
        steepTea();            // spécifique
        pourInCup();           // commun (dupliqué)
        addLemon();            // spécifique
    }
}
```

`boilWater()` et `pourInCup()` sont copiés-collés dans chaque classe. Si la logique de `boilWater()` change (nouvelle température, timer, log), il faut modifier chaque maker. Et rien ne garantit que toutes les classes respectent le même ordre d'étapes.

Avec le Template Method, la classe abstraite fixe le squelette (`prepare()` est `final`) et les sous-classes ne redéfinissent que les étapes spécifiques. Le code commun vit à un seul endroit.
