# Design Pattern : Abstract Factory

Le design pattern Abstract Factory est un pattern de création qui fournit une interface pour créer des familles d'objets liés sans spécifier leurs classes concrètes. Il permet de créer des instances de plusieurs types d'objets connexes sans dépendre de leurs implémentations spécifiques.

**Exemple de Code :**

Imaginons un scénario où nous devons créer des produits dans différentes catégories (par exemple, meubles de bureau et meubles de maison) avec différents styles (moderne ou classique). Le pattern Abstract Factory nous permet de créer des familles de produits cohérents sans détailler leurs implémentations spécifiques.

Voici comment nous pourrions utiliser le design pattern Abstract Factory pour résoudre ce problème :

```java
// Interface pour définir un produit de chaise
interface Chair {
    void sitOn();
}

// Implémentation concrète d'une chaise de bureau
class OfficeChair implements Chair {
    public void sitOn() {
        System.out.println("Vous êtes assis sur une chaise de bureau.");
    }
}

// Implémentation concrète d'une chaise de maison
class HomeChair implements Chair {
    public void sitOn() {
        System.out.println("Vous êtes assis sur une chaise de maison.");
    }
}

// Interface pour définir un produit de table
interface Table {
    void useFor();
}

// Implémentation concrète d'une table de bureau
class OfficeTable implements Table {
    public void useFor() {
        System.out.println("Cette table est utilisée pour le travail de bureau.");
    }
}

// Implémentation concrète d'une table de maison
class HomeTable implements Table {
    public void useFor() {
        System.out.println("Cette table est utilisée à la maison pour diverses activités.");
    }
}

// Interface abstraite pour définir une fabrique abstraite de meubles
interface FurnitureFactory {
    Chair createChair(); // Méthode pour créer une chaise
    Table createTable(); // Méthode pour créer une table
}

// Implémentation concrète d'une fabrique de meubles de bureau
class OfficeFurnitureFactory implements FurnitureFactory {
    public Chair createChair() {
        return new OfficeChair(); // Création d'une chaise de bureau
    }

    public Table createTable() {
        return new OfficeTable(); // Création d'une table de bureau
    }
}

// Implémentation concrète d'une fabrique de meubles de maison
class HomeFurnitureFactory implements FurnitureFactory {
    public Chair createChair() {
        return new HomeChair(); // Création d'une chaise de maison
    }

    public Table createTable() {
        return new HomeTable(); // Création d'une table de maison
    }
}

// Exemple d'utilisation du pattern Abstract Factory
public class Main {
    public static void main(String[] args) {
        // Utilisation de la fabrique de meubles de bureau
        FurnitureFactory officeFactory = new OfficeFurnitureFactory();
        Chair officeChair = officeFactory.createChair();
        Table officeTable = officeFactory.createTable();

        // Utilisation de la fabrique de meubles de maison
        FurnitureFactory homeFactory = new HomeFurnitureFactory();
        Chair homeChair = homeFactory.createChair();
        Table homeTable = homeFactory.createTable();

        // Interactions avec les produits créés
        officeChair.sitOn();
        officeTable.useFor();

        System.out.println(); // Saute une ligne

        homeChair.sitOn();
        homeTable.useFor();
    }
}
```

Dans cet exemple, le design pattern Abstract Factory est utilisé pour définir des familles de produits (chaises et tables) liés à des contextes spécifiques (bureau ou maison). Les interfaces `Chair` et `Table` définissent les fonctionnalités des produits, tandis que les classes concrètes `OfficeChair`, `HomeChair`, `OfficeTable` et `HomeTable` implémentent ces fonctionnalités.

Les fabriques abstraites (`OfficeFurnitureFactory` et `HomeFurnitureFactory`) fournissent des méthodes pour créer des instances cohérentes de chaises et de tables en fonction du contexte (bureau ou maison).

Le pattern Abstract Factory permet de créer des familles de produits sans spécifier les classes concrètes des produits, favorisant ainsi la flexibilité et la séparation des préoccupations dans la conception logicielle.
