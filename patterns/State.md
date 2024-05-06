# Design Pattern : State

Le design pattern State est un pattern comportemental qui permet à un objet de changer son comportement lorsqu'il change d'état interne. Ce pattern encapsule chaque état possible d'un objet dans une classe distincte et permet à l'objet de changer d'état à l'exécution.

**Exemple de Code :**

Imaginons un scénario où nous avons un lecteur de musique qui peut être dans différents états tels que Lecture, Pause ou Arrêt, et le comportement du lecteur varie en fonction de son état actuel.

Voici comment nous pourrions utiliser le design pattern State pour résoudre ce problème :

```java
// Interface représentant un état du lecteur de musique
interface State {
    void play();
    void pause();
    void stop();
}

// Implémentation concrète de l'état de lecture
class PlayingState implements State {
    public void play() {
        System.out.println("Le lecteur est déjà en lecture.");
    }

    public void pause() {
        System.out.println("Pause du lecteur de musique.");
    }

    public void stop() {
        System.out.println("Arrêt du lecteur de musique.");
    }
}

// Implémentation concrète de l'état de pause
class PausedState implements State {
    public void play() {
        System.out.println("Reprise de la lecture du lecteur de musique.");
    }

    public void pause() {
        System.out.println("Le lecteur est déjà en pause.");
    }

    public void stop() {
        System.out.println("Arrêt du lecteur de musique.");
    }
}

// Implémentation concrète de l'état d'arrêt
class StoppedState implements State {
    public void play() {
        System.out.println("Démarrage de la lecture du lecteur de musique.");
    }

    public void pause() {
        System.out.println("Impossible de mettre en pause, le lecteur est arrêté.");
    }

    public void stop() {
        System.out.println("Le lecteur est déjà arrêté.");
    }
}

// Classe représentant le lecteur de musique
class MusicPlayer {
    private State currentState;

    public MusicPlayer() {
        // Initialisation avec l'état d'arrêt par défaut
        this.currentState = new StoppedState();
    }

    public void setState(State state) {
        this.currentState = state;
    }

    public void play() {
        currentState.play();
    }

    public void pause() {
        currentState.pause();
    }

    public void stop() {
        currentState.stop();
    }
}

// Exemple d'utilisation du pattern State
public class Main {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();

        // Lecture lorsque le lecteur est arrêté
        player.play();

        // Passage en état de lecture
        player.setState(new PlayingState());

        // Pause pendant la lecture
        player.pause();

        // Arrêt pendant la lecture
        player.stop();
    }
}
```

Dans cet exemple, le design pattern State est utilisé pour modéliser les différents états d'un lecteur de musique (`PlayingState`, `PausedState`, `StoppedState`). La classe `MusicPlayer` utilise un objet `currentState` qui représente l'état actuel du lecteur. Les méthodes `play()`, `pause()` et `stop()` sont déléguées à l'état actuel, ce qui permet au lecteur de changer de comportement en fonction de son état.

Le pattern State permet de gérer facilement les transitions entre les états et d'encapsuler le comportement spécifique à chaque état dans des classes distinctes, rendant ainsi le code plus modulaire et extensible.
