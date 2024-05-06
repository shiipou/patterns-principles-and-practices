# Design Pattern : Strategy

Le design pattern Strategy est un pattern comportemental qui permet de définir une famille d'algorithmes, d'encapsuler chacun d'eux et de les rendre interchangeables. Ainsi, un client peut choisir l'algorithme à utiliser à tout moment sans modifier le code qui l'utilise.

**Exemple de Code :**

Imaginons un scénario où nous avons une application de tri de listes et nous voulons permettre à l'utilisateur de choisir différents algorithmes de tri (comme le tri bulle, le tri rapide, le tri fusion, etc.) de manière dynamique.

Voici comment nous pourrions utiliser le design pattern Strategy pour résoudre ce problème :

```java
// Interface pour définir un algorithme de tri
interface SortingStrategy {
    void sort(int[] array);
}

// Implémentation concrète d'un algorithme de tri : Tri Bulle
class BubbleSort implements SortingStrategy {
    public void sort(int[] array) {
        System.out.println("Tri bulle en cours...");
        // Implémentation du tri bulle
    }
}

// Implémentation concrète d'un algorithme de tri : Tri Rapide
class QuickSort implements SortingStrategy {
    public void sort(int[] array) {
        System.out.println("Tri rapide en cours...");
        // Implémentation du tri rapide
    }
}

// Contexte utilisant la stratégie de tri
class Sorter {
    private SortingStrategy sortingStrategy;

    public void setSortingStrategy(SortingStrategy sortingStrategy) {
        this.sortingStrategy = sortingStrategy;
    }

    public void performSort(int[] array) {
        if (sortingStrategy != null) {
            sortingStrategy.sort(array); // Utilisation de l'algorithme de tri choisi
        } else {
            System.out.println("Veuillez définir une stratégie de tri.");
        }
    }
}

// Exemple d'utilisation du pattern Strategy
public class Main {
    public static void main(String[] args) {
        Sorter sorter = new Sorter();

        // Utilisation du tri bulle
        sorter.setSortingStrategy(new BubbleSort());
        int[] array1 = {5, 2, 8, 1, 9};
        sorter.performSort(array1);

        // Utilisation du tri rapide
        sorter.setSortingStrategy(new QuickSort());
        int[] array2 = {10, 4, 7, 3, 6};
        sorter.performSort(array2);
    }
}
```

Dans cet exemple, le design pattern Strategy est utilisé pour encapsuler les différents algorithmes de tri (`BubbleSort` et `QuickSort`) dans des classes distinctes implémentant l'interface `SortingStrategy`. La classe `Sorter` utilise une instance de `SortingStrategy` pour exécuter un tri sur un tableau donné en utilisant l'algorithme spécifié.

L'avantage du pattern Strategy est qu'il permet de changer dynamiquement l'algorithme de tri utilisé sans modifier le code de `Sorter` ou du client. Cela rend le code plus flexible, modulaire et extensible.
