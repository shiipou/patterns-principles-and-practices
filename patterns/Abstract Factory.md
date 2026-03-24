# Design Pattern : Abstract Factory

L'Abstract Factory est un pattern de création qui permet de produire des familles d'objets liés entre eux, sans avoir à préciser leurs classes concrètes. On passe par une "fabrique" qui sait créer les bons objets pour un contexte donné.

## Concept fondamental

L'idée est d'avoir une interface de fabrique abstraite qui déclare des méthodes de création pour chaque type de produit de la famille. Chaque variante (bureau, maison, jardin...) fournit sa propre implémentation concrète de cette fabrique. Le code client ne manipule jamais les classes concrètes — il passe par les interfaces de la fabrique et des produits.

La différence avec le Factory Method : ici on crée des *familles entières* de produits liés (chaise + table + canapé...), pas un seul type de produit.

## Exemple

On a besoin de créer des meubles (chaises et tables), mais selon qu'on équipe un bureau ou une maison, les produits sont différents. Plutôt que de gérer ça à coups de `if`, on crée une fabrique par contexte.

```java
interface Chair {
    void sitOn();
}

interface Table {
    void useFor();
}

class OfficeChair implements Chair {
    public void sitOn() {
        System.out.println("Vous êtes assis sur une chaise de bureau.");
    }
}

class HomeChair implements Chair {
    public void sitOn() {
        System.out.println("Vous êtes assis sur une chaise de maison.");
    }
}

class OfficeTable implements Table {
    public void useFor() {
        System.out.println("Cette table est utilisée pour le travail de bureau.");
    }
}

class HomeTable implements Table {
    public void useFor() {
        System.out.println("Cette table est utilisée à la maison.");
    }
}

// La fabrique abstraite : elle sait créer une chaise et une table
interface FurnitureFactory {
    Chair createChair();
    Table createTable();
}

// Fabrique pour le bureau
class OfficeFurnitureFactory implements FurnitureFactory {
    public Chair createChair() { return new OfficeChair(); }
    public Table createTable() { return new OfficeTable(); }
}

// Fabrique pour la maison
class HomeFurnitureFactory implements FurnitureFactory {
    public Chair createChair() { return new HomeChair(); }
    public Table createTable() { return new HomeTable(); }
}

public class Main {
    public static void main(String[] args) {
        FurnitureFactory factory = new OfficeFurnitureFactory();
        Chair chair = factory.createChair();
        Table table = factory.createTable();

        chair.sitOn();
        table.useFor();

        // On change de contexte : on passe à la maison
        factory = new HomeFurnitureFactory();
        chair = factory.createChair();
        table = factory.createTable();

        chair.sitOn();
        table.useFor();
    }
}
```

Le `Main` ne connaît jamais les classes concrètes (`OfficeChair`, `HomeTable`, etc.). Il manipule uniquement l'interface `FurnitureFactory` et les interfaces `Chair` / `Table`. Pour ajouter un nouveau contexte (par ex. "meubles de jardin"), il suffit de créer une nouvelle fabrique — le reste du code ne bouge pas.

## Avantages et inconvénients

**Avantages :**
- Garantit la cohérence entre les produits d'une même famille (pas de chaise bureau avec une table maison)
- Isole le code client des classes concrètes — changer de famille entière revient à changer une seule ligne
- Respecte le principe Open/Closed : on ajoute de nouvelles familles sans modifier le code existant

**Inconvénients :**
- Ajouter un nouveau type de produit à la famille (par ex. un `Sofa`) oblige à modifier toutes les factories existantes
- Peut devenir verbeux quand il y a beaucoup de familles et de types de produits
- Introduit de la complexité si le nombre de familles est faible — un simple Factory Method suffirait

## Sans ce pattern

Sans Abstract Factory, on se retrouve avec des conditions partout pour créer les bons objets :

```java
class FurnitureStore {
    Chair createChair(String style) {
        if (style.equals("office")) return new OfficeChair();
        else if (style.equals("home")) return new HomeChair();
        throw new IllegalArgumentException("Style inconnu");
    }

    Desk createDesk(String style) {
        if (style.equals("office")) return new OfficeDesk();
        else if (style.equals("home")) return new HomeDesk();
        throw new IllegalArgumentException("Style inconnu");
    }
}

// Utilisation — rien n'empêche de mélanger les familles
Chair chair = store.createChair("office");
Desk desk = store.createDesk("home"); // Incohérent, et aucune erreur à la compilation
```

Problèmes :
- Chaque nouveau style oblige à modifier **toutes** les méthodes de création
- Rien ne garantit la cohérence entre les objets — on peut mélanger une `OfficeChair` avec un `HomeDesk` sans que le compilateur bronche
- La logique de sélection (les `if/else`) est dupliquée dans chaque méthode

Avec l'Abstract Factory, on choisit une factory (`OfficeFactory` ou `HomeFactory`) et tous les objets créés sont automatiquement de la même famille. Impossible de mélanger.
