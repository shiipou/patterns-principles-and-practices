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

## Liens avec les autres concepts

**Patterns proches :**
- [Strategy](Strategy.md) — les deux gèrent des variations d'algorithme, mais par des mécanismes opposés. Le Template Method utilise l'**héritage** : la classe parente contrôle le flux et les sous-classes remplissent les trous. Le Strategy utilise la **composition** : on injecte l'algorithme de l'extérieur. Le Strategy est plus flexible (changement à l'exécution), le Template Method est plus structurant (l'ordre des étapes est garanti).
- [Factory Method](Factory%20Method.md) — même mécanique d'héritage. Un Template Method appelle souvent un Factory Method pour créer les objets dont il a besoin dans certaines étapes.

**Principes appliqués :**
- [Don't Repeat Yourself (DRY)](../principles/Don't%20Repeat%20Yourself%20(DRY).md) — c'est **le** pattern anti-duplication. Le code commun (`boilWater`, `pourInCup`) vit une seule fois dans la classe parente, au lieu d'être copié dans chaque sous-classe.
- [Open/Closed](../principles/Open%20&%20Closed.md) — on ajoute de nouvelles boissons (nouvelles sous-classes) sans modifier la classe abstraite ni les sous-classes existantes.
- [Separation of Concerns](../principles/Separation%20of%20Concerns.md) — la structure de l'algorithme (le "quoi" et le "quand") est séparée des détails d'implémentation (le "comment").

**Différence clé avec le Strategy :** si l'**ordre des étapes doit être garanti** et que seuls les détails de certaines étapes varient, c'est le Template Method. Si l'**algorithme entier** doit pouvoir être remplacé librement, c'est le Strategy.
