# Design Pattern : Adapter

L'adaptateur est un design pattern structurel qui permet à des interfaces incompatibles de travailler ensemble. Il permet à des classes avec des interfaces incompatibles de collaborer en les enveloppant avec un adaptateur qui offre une interface commune.

**Exemple de Code :**

Imaginons un scénario où nous avons une ancienne bibliothèque de manipulation de fichiers qui utilise une interface `FileReader` avec une méthode `readFile()`, mais nous voulons utiliser une nouvelle bibliothèque avec une interface différente `NewFileReader` avec une méthode `openFile()` et `readContent()`.

Voici comment nous pourrions utiliser le design pattern Adapter pour résoudre ce problème :

```java
// Interface de la vieille bibliothèque
interface FileReader {
    String readFile();
}

// Classe implémentant l'ancienne interface
class OldFileReader implements FileReader {
    public String readFile() {
        return "Contenu du fichier lu par l'ancienne bibliothèque";
    }
}

// Interface de la nouvelle bibliothèque
interface NewFileReader {
    void openFile();
    String readContent();
}

// Classe adaptateur pour utiliser la nouvelle bibliothèque avec l'ancienne interface
class Adapter implements FileReader {
    private NewFileReader newFileReader;

    public Adapter(NewFileReader newFileReader) {
        this.newFileReader = newFileReader;
    }

    public String readFile() {
        newFileReader.openFile(); // Appelle la méthode de la nouvelle bibliothèque
        return newFileReader.readContent(); // Appelle la méthode de la nouvelle bibliothèque
    }
}

// Utilisation de l'adaptateur
public class Main {
    public static void main(String[] args) {
        NewFileReader newReader = new NewFileReaderImpl(); // Nouvelle bibliothèque
        FileReader adapter = new Adapter(newReader); // Adaptateur
        String content = adapter.readFile(); // Utilisation de l'ancienne interface avec la nouvelle bibliothèque
        System.out.println("Contenu du fichier lu : " + content);
    }
}
```

Dans cet exemple, l'interface `Adapter` permet à la classe `Adapter` d'utiliser la nouvelle bibliothèque `NewFileReader` en lui fournissant une interface compatible avec l'ancienne bibliothèque `FileReader`.

Cela montre comment le pattern Adapter peut être utilisé pour rendre des classes incompatibles compatibles en encapsulant la complexité de l'adaptation dans un adaptateur.
