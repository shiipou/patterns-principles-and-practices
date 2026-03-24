# Design Pattern : Strategy

Le Strategy permet de définir une famille d'algorithmes interchangeables. On encapsule chaque algorithme dans sa propre classe et on laisse le code client choisir lequel utiliser — sans `if/else` ni `switch`.

## Concept fondamental

On définit une interface commune pour tous les algorithmes de la famille. Chaque algorithme est implémenté dans une classe concrète. Le contexte (la classe qui utilise l'algorithme) possède une référence vers cette interface et délègue le travail. On peut changer de stratégie à tout moment via un setter, même à l'exécution.

La différence avec le State : le State change de comportement automatiquement (en interne), le Strategy est choisi explicitement par le client.

## Exemple

Une appli de navigation propose plusieurs modes de transport. Selon le mode choisi, le calcul d'itinéraire est différent.

```java
interface RouteStrategy {
    void calculateRoute(String from, String to);
}

class CarRoute implements RouteStrategy {
    public void calculateRoute(String from, String to) {
        System.out.println("🚗 Route en voiture de " + from + " à " + to);
    }
}

class BikeRoute implements RouteStrategy {
    public void calculateRoute(String from, String to) {
        System.out.println("🚲 Itinéraire vélo de " + from + " à " + to);
    }
}

class WalkRoute implements RouteStrategy {
    public void calculateRoute(String from, String to) {
        System.out.println("🚶 Trajet à pied de " + from + " à " + to);
    }
}

class Navigator {
    private RouteStrategy strategy;

    public void setStrategy(RouteStrategy strategy) {
        this.strategy = strategy;
    }

    public void navigate(String from, String to) {
        strategy.calculateRoute(from, to);
    }
}

public class Main {
    public static void main(String[] args) {
        Navigator nav = new Navigator();

        nav.setStrategy(new CarRoute());
        nav.navigate("Paris", "Lyon");

        nav.setStrategy(new BikeRoute());
        nav.navigate("Paris", "Versailles");
    }
}
```

Le `Navigator` ne connaît aucun détail de calcul — il délègue tout à la stratégie. On peut changer de stratégie à la volée, et en ajouter de nouvelles (transport en commun, trottinette...) sans toucher au code existant.

## Avantages et inconvénients

**Avantages :**
- Permet de changer d'algorithme dynamiquement à l'exécution
- Chaque algorithme est isolé dans sa propre classe — facile à tester et à maintenir
- Respecte le principe Open/Closed : ajouter une stratégie ne modifie rien d'existant
- Élimine les chaînes de `if/else` pour choisir un algorithme

**Inconvénients :**
- Multiplie les classes si on a beaucoup de stratégies
- Le client doit connaître les différentes stratégies disponibles pour choisir la bonne
- Peut être du sur-engineering si l'algorithme ne change jamais

## Sans ce pattern

Sans Strategy, tous les algorithmes sont dans la même classe :

```java
class Navigator {
    public Route calculateRoute(String from, String to, String mode) {
        if (mode.equals("car")) {
            // 50 lignes : routes principales, autoroutes, péages...
        } else if (mode.equals("bike")) {
            // 40 lignes : pistes cyclables, dénivelé...
        } else if (mode.equals("walk")) {
            // 30 lignes : chemins piétons, parcs, escaliers...
        }
        // Nouveau mode de transport → on touche cette méthode
    }
}
```

La classe `Navigator` grossit à chaque nouveau mode de transport. Modifier l'algo vélo risque de casser celui de la voiture (même méthode). Et il est impossible de tester un algorithme sans instancier tout le `Navigator`.

Avec Strategy, chaque algorithme est une classe isolée. On peut les tester, les modifier et en ajouter indépendamment.
