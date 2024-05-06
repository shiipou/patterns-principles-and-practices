# Design Pattern : Observer

Le design pattern Observer est un pattern comportemental qui permet à un objet (appelé sujet) de maintenir une liste de dépendances (observateurs) afin de les notifier automatiquement en cas de changement d'état ou de données.

**Exemple de Code :**

Imaginons un scénario où nous avons un sujet (Subject) représentant un objet qui peut être observé par plusieurs observateurs (Observers). Lorsque l'état du sujet change, tous les observateurs sont notifiés et mis à jour.

Voici comment nous pourrions utiliser le design pattern Observer pour résoudre ce problème :

```java
import java.util.ArrayList;
import java.util.List;

// Interface pour définir un observateur
interface Observer {
    void update(); // Méthode appelée lorsque le sujet notifie un changement
}

// Classe concrète représentant un observateur
class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update() {
        System.out.println(name + " : Notification reçue du sujet.");
        // Effectuer des actions en réponse à la notification
    }
}

// Interface pour définir un sujet observable
interface Subject {
    void addObserver(Observer observer); // Méthode pour ajouter un observateur
    void removeObserver(Observer observer); // Méthode pour supprimer un observateur
    void notifyObservers(); // Méthode pour notifier tous les observateurs
}

// Classe concrète représentant un sujet observable
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        System.out.println("Notifying observers...");
        for (Observer observer : observers) {
            observer.update(); // Appeler la méthode update() de chaque observateur
        }
    }

    // Méthode pour simuler un changement d'état
    public void stateChanged() {
        // Effectuer des actions pour changer l'état du sujet
        notifyObservers(); // Notifier les observateurs du changement
    }
}

// Exemple d'utilisation du pattern Observer
public class Main {
    public static void main(String[] args) {
        // Création d'un sujet observable
        ConcreteSubject subject = new ConcreteSubject();

        // Création de plusieurs observateurs
        Observer observer1 = new ConcreteObserver("Observer 1");
        Observer observer2 = new ConcreteObserver("Observer 2");

        // Ajout des observateurs au sujet observable
        subject.addObserver(observer1);
        subject.addObserver(observer2);

        // Simulation d'un changement d'état du sujet
        subject.stateChanged();
    }
}
```

Dans cet exemple, le design pattern Observer est utilisé pour permettre à un sujet observable (`ConcreteSubject`) de maintenir une liste dynamique d'observateurs (`ConcreteObserver`). Lorsque l'état du sujet change (simulé par `stateChanged()`), le sujet notifie tous les observateurs en appelant la méthode `update()` sur chaque observateur.

Le pattern Observer permet d'établir des relations lâches entre les objets, où le sujet n'a pas besoin de connaître les détails spécifiques des observateurs. Cela favorise la modularité et l'évolutivité du code.
