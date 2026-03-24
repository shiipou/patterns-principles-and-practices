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

## Liens avec les autres concepts

**Patterns proches :**
- [State](State.md) — structure quasi identique, mais le Strategy est **choisi par le client** ("utilise l'algo vélo"), tandis que le State **change automatiquement** selon l'état interne de l'objet. Le Strategy est un choix conscient, le State est une machine à états.
- [Template Method](Template%20Method.md) — les deux gèrent des variations d'algorithme, mais le Template Method utilise l'**héritage** (la classe parente fixe le squelette), tandis que le Strategy utilise la **composition** (on injecte l'algorithme). Le Strategy est plus flexible : on peut changer d'algorithme à l'exécution.
- [Decorator](Decorator.md) — le Decorator **empile** des comportements (email + SMS + log), le Strategy **substitue** un algorithme par un autre (voiture OU vélo). Le Decorator est additif, le Strategy est alternatif.

**Principes appliqués :**
- [Open/Closed](../principles/Open%20&%20Closed.md) — ajouter une nouvelle stratégie (`ScooterRoute`) ne modifie ni le `Navigator` ni les stratégies existantes.
- [Dependency Inversion](../principles/Dependency%20Inversion.md) — le `Navigator` dépend de l'interface `RouteStrategy`, pas des classes concrètes.
- [Loose Coupling](../principles/Loose%20Coupling.md) — le contexte et les stratégies sont indépendants, reliés uniquement par l'interface.
- [Single Responsibility](../principles/Single%20Responsibility.md) — chaque stratégie a une seule responsabilité : calculer un type d'itinéraire.

**Pratiques liées :**
- [Automated Testing](../practices/Automated%20Testing.md) — chaque stratégie est testable en isolation, sans instancier le `Navigator`.
