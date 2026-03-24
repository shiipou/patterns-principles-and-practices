# Design Pattern : Factory Method

Le Factory Method délègue la création d'objets à des sous-classes. La classe parente définit une méthode abstraite "fabrique" que chaque sous-classe implémente pour créer le bon type d'objet. Le code appelant manipule l'abstraction sans jamais savoir quel type concret est instancié.

## Concept fondamental

La classe parente (la "fabrique") définit un algorithme ou un workflow qui utilise des objets, mais elle ne sait pas quels objets concrets elle manipule — c'est la méthode `createDocument()` (la "factory method") qui est abstraite et laissée aux sous-classes. Chaque sous-classe décide quel type concret créer.

La différence avec l'Abstract Factory : ici on ne crée qu'un seul type de produit à la fois, pas une famille entière.

## Exemple

On a besoin de créer des documents (PDF, HTML), mais c'est la sous-classe de la fabrique qui décide quel type créer.

```java
interface Document {
    void open();
    void close();
}

class PdfDocument implements Document {
    public void open() { System.out.println("Ouverture du document PDF"); }
    public void close() { System.out.println("Fermeture du document PDF"); }
}

class HtmlDocument implements Document {
    public void open() { System.out.println("Ouverture du document HTML"); }
    public void close() { System.out.println("Fermeture du document HTML"); }
}

// La classe parente définit le squelette, mais délègue la création
abstract class DocumentFactory {
    public abstract Document createDocument();

    public void manipulateDocument() {
        Document document = createDocument();
        document.open();
        document.close();
    }
}

class PdfDocumentFactory extends DocumentFactory {
    public Document createDocument() { return new PdfDocument(); }
}

class HtmlDocumentFactory extends DocumentFactory {
    public Document createDocument() { return new HtmlDocument(); }
}

public class Main {
    public static void main(String[] args) {
        DocumentFactory pdfFactory = new PdfDocumentFactory();
        pdfFactory.manipulateDocument();

        DocumentFactory htmlFactory = new HtmlDocumentFactory();
        htmlFactory.manipulateDocument();
    }
}
```

Le `Main` ne sait pas si c'est un PDF ou du HTML qui est créé — il appelle juste `manipulateDocument()`. Pour supporter un nouveau format (Markdown par ex.), il suffit de créer une nouvelle fabrique et un nouveau `Document`. Le code existant ne bouge pas.

## Avantages et inconvénients

**Avantages :**
- Découple le code de création des objets du code qui les utilise
- Respecte le principe Open/Closed : on ajoute un nouveau type en ajoutant une sous-classe, sans modifier l'existant
- Le code client est plus flexible — il manipule des abstractions, pas des classes concrètes

**Inconvénients :**
- Crée une hiérarchie de classes parallèle (une factory par type de produit) qui peut devenir lourde
- Peut être du sur-engineering si les types de produits ne changent jamais
- Plus complexe qu'un simple `new` quand on n'a que 2-3 types figés

## Sans ce pattern

Sans Factory Method, le code client est rempli de conditionnelles de création :

```java
class DocumentService {
    public Document createDocument(String type) {
        if (type.equals("pdf")) {
            PdfDocument doc = new PdfDocument();
            doc.setRenderer(new PdfRenderer());
            doc.setMargins(2.5, 2.5, 2.5, 2.5);
            return doc;
        } else if (type.equals("html")) {
            HtmlDocument doc = new HtmlDocument();
            doc.setCss("default.css");
            return doc;
        }
        // Chaque nouveau type → on ajoute un else if ici
        throw new IllegalArgumentException("Type inconnu: " + type);
    }
}
```

Chaque nouveau type de document oblige à modifier cette méthode (violation du Open/Closed). La logique de création est mélangée avec la logique métier, et tester `DocumentService` sans instancier de vrais `PdfDocument` est impossible.

Avec le Factory Method, chaque type a sa propre factory. Ajouter un nouveau format = ajouter une classe, sans toucher au code existant.

## Liens avec les autres concepts

**Patterns proches :**
- [Abstract Factory](Abstract%20Factory.md) — l'Abstract Factory utilise souvent des Factory Methods en interne. La différence : le Factory Method crée **un seul type** de produit, l'Abstract Factory crée des **familles entières** de produits liés.
- [Template Method](Template%20Method.md) — même mécanique d'héritage. Le Template Method délègue des **étapes d'un algorithme** aux sous-classes, le Factory Method délègue la **création d'objets**. Souvent, un Template Method contient un Factory Method.
- [Strategy](Strategy.md) — le Strategy utilise la **composition** pour varier le comportement, le Factory Method utilise l'**héritage**. Le Strategy est plus flexible (changement à l'exécution), le Factory Method est plus simple.

**Principes appliqués :**
- [Open/Closed](../principles/Open%20&%20Closed.md) — ajouter un nouveau type de produit = créer une nouvelle sous-classe, sans modifier le code existant.
- [Dependency Inversion](../principles/Dependency%20Inversion.md) — le code client dépend de l'abstraction `Document`, pas des classes concrètes `PdfDocument` ou `HtmlDocument`.
- [Single Responsibility](../principles/Single%20Responsibility.md) — la logique de création est séparée de la logique d'utilisation.

**Différence clé avec l'Abstract Factory :** si tu n'as qu'**un seul type** de produit à créer (des documents, des notifications, des connexions…), le Factory Method suffit. Si tu as **plusieurs types liés** qui doivent être cohérents entre eux (chaise + table du même style), c'est l'Abstract Factory qu'il te faut.
