# Design Pattern : Observer

L'Observer permet à un objet (le "sujet") de prévenir automatiquement une liste d'objets intéressés (les "observateurs") quand son état change. Le sujet n'a pas besoin de savoir ce que les observateurs font de l'info — il les notifie, point.

## Concept fondamental

Le sujet maintient une liste d'observateurs et expose des méthodes pour s'y abonner (`addObserver`) ou s'en désabonner (`removeObserver`). Quand son état change, il appelle `notifyObservers()` qui parcourt la liste et prévient chaque observateur via une méthode commune (`update`). Le sujet et les observateurs sont faiblement couplés : le sujet ne connaît que l'interface `Observer`, pas les implémentations concrètes.

## Exemple

On a un capteur de température. Quand la température change, on veut que plusieurs affichages se mettent à jour automatiquement.

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(float temperature);
}

interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

class TemperatureSensor implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private float temperature;

    public void addObserver(Observer observer) { observers.add(observer); }
    public void removeObserver(Observer observer) { observers.remove(observer); }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature);
        }
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        notifyObservers();
    }
}

class PhoneDisplay implements Observer {
    public void update(float temperature) {
        System.out.println("📱 Téléphone : " + temperature + "°C");
    }
}

class DashboardDisplay implements Observer {
    public void update(float temperature) {
        System.out.println("🖥️ Dashboard : " + temperature + "°C");
    }
}

public class Main {
    public static void main(String[] args) {
        TemperatureSensor sensor = new TemperatureSensor();

        sensor.addObserver(new PhoneDisplay());
        sensor.addObserver(new DashboardDisplay());

        sensor.setTemperature(22.5f);  // Les deux affichages se mettent à jour
        sensor.setTemperature(25.0f);  // Encore une fois
    }
}
```

Le capteur ne connaît pas les détails des affichages. Il sait juste qu'il a des observateurs à prévenir. On peut en ajouter ou en retirer à tout moment sans toucher au code du capteur.

## Avantages et inconvénients

**Avantages :**
- Couplage lâche entre le sujet et les observateurs — ils communiquent par interface
- On peut ajouter ou retirer des observateurs à la volée, sans modifier le sujet
- Favorise le principe Open/Closed : nouveaux observateurs sans toucher au code existant

**Inconvénients :**
- L'ordre de notification n'est pas garanti
- Peut provoquer des cascades de mises à jour difficiles à déboguer
- Si les observateurs ne se désinscrivent pas, risque de fuite mémoire (le sujet garde une référence)
- Le sujet envoie la même notification à tous les observateurs, même si certains n'en ont pas besoin

## Sans ce pattern

Sans Observer, le sujet doit connaître explicitement chacun de ses affichages :

```java
class TemperatureSensor {
    private PhoneDisplay phone;
    private DashboardDisplay dashboard;
    private LogService logger;

    public void setTemperature(float temp) {
        this.temperature = temp;
        phone.updateScreen(temp);       // couplage direct
        dashboard.refresh(temp);         // couplage direct
        logger.log("temp=" + temp);      // couplage direct
    }
}
```

Chaque nouvel affichage oblige à modifier `TemperatureSensor` : ajouter un champ, ajouter un appel dans `setTemperature()`. Le capteur — qui ne devrait se soucier que de la température — connaît les détails de chaque affichage. Impossible d'ajouter un nouvel observateur sans recompiler le capteur.

Avec l'Observer, le capteur ne connaît qu'une interface `Observer`. N'importe quel objet peut s'abonner ou se désabonner à tout moment, sans que le capteur n'en sache rien.
