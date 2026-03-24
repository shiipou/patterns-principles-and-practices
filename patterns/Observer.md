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

## Liens avec les autres concepts

**Patterns proches :**
- [Strategy](Strategy.md) — les deux utilisent le polymorphisme et des interfaces, mais le Strategy est une relation **1-vers-1** (un contexte utilise une stratégie), tandis que l'Observer est une relation **1-vers-N** (un sujet notifie plusieurs observateurs).
- [State](State.md) — le State change l'état interne d'un objet, ce qui peut déclencher des notifications Observer. Les deux patterns se combinent bien : un changement d'état notifie les observateurs.

**Principes appliqués :**
- [Loose Coupling](../principles/Loose%20Coupling.md) — c'est **le** pattern du couplage lâche. Le sujet ne connaît que l'interface `Observer`, pas les implémentations concrètes. Ajouter ou retirer un observateur ne modifie rien au sujet.
- [Open/Closed](../principles/Open%20&%20Closed.md) — on ajoute de nouveaux observateurs sans toucher au code du sujet.
- [Separation of Concerns](../principles/Separation%20of%20Concerns.md) — le sujet s'occupe de son état, chaque observateur s'occupe de sa réaction. Le capteur de température ne sait rien de l'affichage.

**Pratiques liées :**
- [Automated Testing](../practices/Automated%20Testing.md) — les observateurs sont faciles à tester en isolation puisqu'ils ne dépendent que d'une interface simple (`update`).
