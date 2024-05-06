# Design Pattern : Factory Method

Le design pattern Factory Method est un pattern de création qui fournit une interface pour créer des objets dans une classe parente, mais permet aux sous-classes de modifier le type d'objets créés.

**Exemple de Code :**

Imaginons un scénario où nous devons créer des documents de différents types (par exemple, PDF, HTML) en fonction d'une demande, tout en permettant à nos sous-classes de décider du type de document à créer.

Voici comment nous pourrions utiliser le design pattern Factory Method pour résoudre ce problème :

```java
// Interface pour définir un document
interface Document {
    void open();
    void close();
}

// Implémentation concrète d'un document PDF
class PdfDocument implements Document {
    public void open() {
        System.out.println("Ouverture du document PDF");
    }

    public void close() {
        System.out.println("Fermeture du document PDF");
    }
}

// Implémentation concrète d'un document HTML
class HtmlDocument implements Document {
    public void open() {
        System.out.println("Ouverture du document HTML");
    }

    public void close() {
        System.out.println("Fermeture du document HTML");
    }
}

// Classe abstraite représentant une fabrique de documents
abstract class DocumentFactory {
    // Méthode factory (fabrique) pour créer un document
    public abstract Document createDocument();

    // Méthode utilitaire pour manipuler le document créé
    public void manipulateDocument() {
        Document document = createDocument(); // Appel de la méthode factory
        document.open();
        // Manipulation supplémentaire du document
        document.close();
    }
}

// Fabrique concrète pour créer des documents PDF
class PdfDocumentFactory extends DocumentFactory {
    public Document createDocument() {
        return new PdfDocument(); // Création d'un document PDF
    }
}

// Fabrique concrète pour créer des documents HTML
class HtmlDocumentFactory extends DocumentFactory {
    public Document createDocument() {
        return new HtmlDocument(); // Création d'un document HTML
    }
}

// Exemple d'utilisation du pattern Factory Method
public class Main {
    public static void main(String[] args) {
        // Utilisation de la fabrique pour créer et manipuler un document PDF
        DocumentFactory pdfFactory = new PdfDocumentFactory();
        pdfFactory.manipulateDocument();

        System.out.println(); // Saute une ligne

        // Utilisation de la fabrique pour créer et manipuler un document HTML
        DocumentFactory htmlFactory = new HtmlDocumentFactory();
        htmlFactory.manipulateDocument();
    }
}
```

Dans cet exemple, le design pattern Factory Method est utilisé pour déléguer la création d'objets (`Document`) à des sous-classes (`PdfDocumentFactory`, `HtmlDocumentFactory`). La classe abstraite `DocumentFactory` définit une méthode `createDocument()` qui est implémentée par les sous-classes pour créer des instances spécifiques de `Document` (PDF ou HTML).

Le pattern Factory Method permet de déléguer la responsabilité de création d'objets aux sous-classes, ce qui rend le code plus flexible et extensible. Les clients utilisent simplement les méthodes des fabriques pour créer et manipuler des objets sans se soucier de leur implémentation concrète.
