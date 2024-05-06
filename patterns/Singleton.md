# Design Pattern : Singleton

Le Singleton est un design pattern de création qui garantit qu'une classe n'a qu'une seule instance et fournit un point d'accès global à cette instance.

**Exemple de Code :**

Voici un exemple d'implémentation du Singleton en Java :

```java
public class Singleton {
    // Instance unique privée
    private static Singleton instance;

    // Constructeur privé pour empêcher l'instanciation directe depuis l'extérieur
    private Singleton() {
        // Initialisation de la classe Singleton
    }

    // Méthode statique pour obtenir l'instance unique
    public static Singleton getInstance() {
        // Créer l'instance si elle n'existe pas encore
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    // Méthode utilitaire de la classe Singleton
    public void doSomething() {
        System.out.println("Singleton instance doing something...");
    }
}

// Exemple d'utilisation du Singleton
public class Main {
    public static void main(String[] args) {
        // Obtention de l'instance unique du Singleton
        Singleton singleton = Singleton.getInstance();

        // Utilisation de l'instance Singleton
        singleton.doSomething();

        // Tentative d'instanciation directe (non possible car le constructeur est privé)
        // Singleton anotherInstance = new Singleton(); // Erreur de compilation

        // Obtention de la même instance à partir de n'importe où dans le code
        Singleton anotherSingleton = Singleton.getInstance();
        anotherSingleton.doSomething(); // Utilisation de la même instance
    }
}
```

Dans cet exemple, la classe `Singleton` possède une instance statique privée et un constructeur privé, ce qui empêche l'instanciation directe depuis l'extérieur de la classe. La méthode `getInstance()` fournit le point d'accès global à l'instance unique du Singleton. La première fois que `getInstance()` est appelée, elle crée une nouvelle instance de `Singleton`, et les appels suivants renvoient simplement cette instance déjà créée.

Le pattern Singleton est couramment utilisé lorsque vous avez besoin d'une seule instance partagée dans toute l'application, comme un gestionnaire de connexions à une base de données ou un gestionnaire de configuration.
