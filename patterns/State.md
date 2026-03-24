# Design Pattern : State

Le State permet à un objet de changer de comportement quand son état interne change. Au lieu de caser des `if/else` ou des `switch` partout, on encapsule chaque état dans sa propre classe.

## Concept fondamental

Chaque état possible est représenté par une classe qui implémente une interface commune. L'objet principal (le "contexte") possède une référence vers son état actuel et lui délègue toutes les actions. Quand l'état change, on remplace cette référence par un nouvel objet d'état. Le comportement de l'objet change donc automatiquement, sans un seul `if`.

Chaque classe d'état gère aussi ses propres transitions : c'est l'état lui-même qui décide dans quel autre état on passe, pas le contexte.

## Exemple

Un lecteur de musique peut être en lecture, en pause ou à l'arrêt. Selon l'état, appuyer sur "play" ne fait pas la même chose.

```java
interface State {
    void play(MusicPlayer player);
    void pause(MusicPlayer player);
    void stop(MusicPlayer player);
}

class StoppedState implements State {
    public void play(MusicPlayer player) {
        System.out.println("▶ Démarrage de la lecture.");
        player.setState(new PlayingState());
    }
    public void pause(MusicPlayer player) {
        System.out.println("⏸ Impossible, le lecteur est arrêté.");
    }
    public void stop(MusicPlayer player) {
        System.out.println("⏹ Déjà arrêté.");
    }
}

class PlayingState implements State {
    public void play(MusicPlayer player) {
        System.out.println("▶ Déjà en lecture.");
    }
    public void pause(MusicPlayer player) {
        System.out.println("⏸ Mise en pause.");
        player.setState(new PausedState());
    }
    public void stop(MusicPlayer player) {
        System.out.println("⏹ Arrêt du lecteur.");
        player.setState(new StoppedState());
    }
}

class PausedState implements State {
    public void play(MusicPlayer player) {
        System.out.println("▶ Reprise de la lecture.");
        player.setState(new PlayingState());
    }
    public void pause(MusicPlayer player) {
        System.out.println("⏸ Déjà en pause.");
    }
    public void stop(MusicPlayer player) {
        System.out.println("⏹ Arrêt du lecteur.");
        player.setState(new StoppedState());
    }
}

class MusicPlayer {
    private State currentState;

    public MusicPlayer() {
        this.currentState = new StoppedState();
    }

    public void setState(State state) { this.currentState = state; }
    public void play() { currentState.play(this); }
    public void pause() { currentState.pause(this); }
    public void stop() { currentState.stop(this); }
}

public class Main {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();

        player.play();   // ▶ Démarrage de la lecture.
        player.pause();  // ⏸ Mise en pause.
        player.play();   // ▶ Reprise de la lecture.
        player.stop();   // ⏹ Arrêt du lecteur.
        player.pause();  // ⏸ Impossible, le lecteur est arrêté.
    }
}
```

Chaque état gère ses propres transitions. Le `MusicPlayer` délègue tout à `currentState` sans se préoccuper de quel état il est. Pour ajouter un nouvel état (par ex. "Buffering"), il suffit de créer une nouvelle classe — pas besoin de toucher aux autres.

## Avantages et inconvénients

**Avantages :**
- Élimine les longues chaînes de `if/else` ou `switch` liées à l'état
- Chaque état est isolé dans sa propre classe — facile à lire et à maintenir
- Ajouter un nouvel état ne nécessite pas de modifier les états existants (Open/Closed)
- Les transitions sont explicites et documentées dans le code

**Inconvénients :**
- Peut être du sur-engineering si l'objet n'a que 2-3 états simples
- Multiplie le nombre de classes dans le projet
- Les transitions dispersées dans chaque classe d'état peuvent être plus difficiles à suivre qu'une table de transitions centralisée

## Sans ce pattern

Sans le pattern State, chaque méthode répète le même bloc de conditions :

```java
class MusicPlayer {
    private String state = "stopped";

    public void pressPlay() {
        if (state.equals("stopped")) {
            startPlayback();
            state = "playing";
        } else if (state.equals("playing")) {
            // rien
        } else if (state.equals("paused")) {
            resumePlayback();
            state = "playing";
        }
    }

    public void pressStop() {
        if (state.equals("stopped")) {
            // rien
        } else if (state.equals("playing")) {
            stopPlayback();
            state = "stopped";
        } else if (state.equals("paused")) {
            stopPlayback();
            state = "stopped";
        }
    }

    // ... et pareil pour pressPause(), pressNext(), etc.
}
```

Chaque méthode duplique le même switch sur l'état. Ajouter un nouvel état ("buffering", "recording") oblige à modifier **toutes** les méthodes. À 5 états et 6 méthodes, on a 30 blocs conditionnels à maintenir — et un seul oubli crée un bug.

Avec le pattern State, chaque état est une classe. Ajouter un état = ajouter une classe, sans toucher aux autres.
