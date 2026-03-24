# Design Pattern : Adapter

L'Adapter est un pattern structurel qui permet de faire collaborer deux interfaces incompatibles. On enveloppe une classe existante dans un adaptateur qui expose l'interface attendue par le reste du code.

## Concept fondamental

L'adaptateur agit comme un traducteur entre deux interfaces. Il implémente l'interface que le code client attend, et en interne, il délègue les appels à l'objet "adapté" qui a une interface différente. Le code client ne sait même pas qu'il communique avec un objet adapté — il voit juste une interface qu'il connaît.

Il existe deux variantes : l'adaptateur par composition (on wrape l'objet) et l'adaptateur par héritage (on étend la classe). En pratique, la composition est largement préférée.

## Exemple

On a une vieille bibliothèque qui lit des fichiers via une interface `FileReader` avec une méthode `readFile()`. On veut migrer vers une nouvelle lib qui a une interface différente (`openFile()` + `readContent()`), mais sans toucher au code qui utilise `FileReader`.

```java
// L'ancienne interface que tout le code utilise
interface FileReader {
    String readFile();
}

class OldFileReader implements FileReader {
    public String readFile() {
        return "Contenu lu par l'ancienne bibliothèque";
    }
}

// La nouvelle lib avec une interface différente
interface NewFileReader {
    void openFile();
    String readContent();
}

// L'adaptateur : il implémente l'ancienne interface
// mais délègue le travail à la nouvelle lib
class Adapter implements FileReader {
    private NewFileReader newFileReader;

    public Adapter(NewFileReader newFileReader) {
        this.newFileReader = newFileReader;
    }

    public String readFile() {
        newFileReader.openFile();
        return newFileReader.readContent();
    }
}

public class Main {
    public static void main(String[] args) {
        NewFileReader newReader = new NewFileReaderImpl();
        FileReader adapter = new Adapter(newReader);

        // Le code appelant ne sait pas qu'il utilise la nouvelle lib
        String content = adapter.readFile();
        System.out.println("Contenu : " + content);
    }
}
```

Le code appelant continue d'utiliser `FileReader` comme avant. L'`Adapter` traduit les appels vers la nouvelle bibliothèque en coulisses.

## Avantages et inconvénients

**Avantages :**
- Permet de réutiliser du code existant sans le modifier
- Respecte le principe Open/Closed : on ajoute un adaptateur sans toucher aux classes existantes
- Isole la complexité de l'adaptation dans une seule classe

**Inconvénients :**
- Ajoute une couche d'indirection qui peut compliquer le débogage
- Peut masquer des incompatibilités fondamentales entre les deux interfaces (conversion de données lossy, etc.)
- Si on multiplie les adaptateurs, c'est souvent le signe qu'il faut repenser l'architecture

## Sans ce pattern

Sans Adapter, le code client est collé à l'API spécifique d'une librairie :

```java
class ReportGenerator {
    private OldXmlParser parser = new OldXmlParser();

    public Report generate(String filePath) {
        XmlNode root = parser.readXml(filePath);
        List<XmlNode> rows = parser.findNodes(root, "//data/row");
        for (XmlNode row : rows) {
            String value = parser.getAttr(row, "value");
            // ... traitement spécifique à l'API de OldXmlParser
        }
    }
}
```

Le jour où on veut passer à `NewJsonParser`, il faut réécrire toute la classe `ReportGenerator` — chaque appel à `readXml`, `findNodes`, `getAttr` doit être remplacé. Et si d'autres classes utilisent aussi `OldXmlParser`, c'est le même travail partout.

Avec un Adapter, on crée une couche de traduction entre l'interface qu'on attend (`FileReader`) et la librairie réelle. Le jour où on change de lib, on remplace un seul adaptateur — le reste du code ne bouge pas.
