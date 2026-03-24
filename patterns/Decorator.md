# Design Pattern : Decorator

Le Decorator est un pattern structurel qui permet d'ajouter des comportements à un objet existant sans toucher à sa classe. Plutôt que de multiplier les sous-classes pour chaque combinaison possible, on empile des "couches" autour de l'objet de base par composition.

## Concept fondamental

Un décorateur implémente la même interface que l'objet qu'il enveloppe. Il reçoit l'objet original dans son constructeur et lui délègue les appels, en ajoutant son propre comportement avant ou après. Puisque chaque décorateur a la même interface que l'objet de base, on peut les empiler à l'infini — chaque couche ajoute une responsabilité sans modifier les autres.

C'est de la composition plutôt que de l'héritage : au lieu d'une classe `EmailSMSLogRetryNotifier`, on compose `Email(SMS(Log(Retry(base))))`.

## Exemple

On part d'un système de notification. Un `MessageNotifier` sait juste afficher un message. On veut pouvoir y greffer l'envoi par email, par SMS, du logging, du retry, etc. — sans modifier la classe d'origine.

```java
interface Notifier {
    void send(String message);
}

// La base : affiche juste le message
class MessageNotifier implements Notifier {
    public void send(String message) {
        System.out.println("[MESSAGE] " + message);
    }
}

// Classe parente des décorateurs
abstract class NotifierDecorator implements Notifier {
    protected Notifier wrappedNotifier;

    public NotifierDecorator(Notifier wrappedNotifier) {
        this.wrappedNotifier = wrappedNotifier;
    }

    public void send(String message) {
        wrappedNotifier.send(message);
    }
}

// Envoie aussi par email
class EmailDecorator extends NotifierDecorator {
    public EmailDecorator(Notifier wrappedNotifier) {
        super(wrappedNotifier);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("[EMAIL] Envoi : " + message);
    }
}

// Envoie aussi par SMS
class SMSDecorator extends NotifierDecorator {
    public SMSDecorator(Notifier wrappedNotifier) {
        super(wrappedNotifier);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("[SMS] Envoi : " + message);
    }
}

// Ajoute du logging avant/après l'envoi
class LogDecorator extends NotifierDecorator {
    public LogDecorator(Notifier wrappedNotifier) {
        super(wrappedNotifier);
    }

    public void send(String message) {
        System.out.println("[LOG] Notification en cours : " + message);
        super.send(message);
        System.out.println("[LOG] Notification terminée.");
    }
}

// Relance automatiquement en cas d'échec
class RetryDecorator extends NotifierDecorator {
    private int attempts;
    private long delay;

    public RetryDecorator(Notifier wrappedNotifier, int attempts, long delay) {
        super(wrappedNotifier);
        this.attempts = attempts;
        this.delay = delay;
    }

    public void send(String message) {
        for (int i = 1; i <= attempts; i++) {
            try {
                super.send(message);
                return;
            } catch (Exception e) {
                System.out.println("[RETRY] Tentative " + i + "/" + attempts + " échouée, retry dans " + delay + "ms...");
                try { Thread.sleep(delay); } catch (InterruptedException ie) { Thread.currentThread().interrupt(); }
            }
        }
    }
}

// Exécute l'envoi dans un thread séparé
class AsyncDecorator extends NotifierDecorator {
    public AsyncDecorator(Notifier wrappedNotifier) {
        super(wrappedNotifier);
    }

    public void send(String message) {
        new Thread(() -> {
            System.out.println("[ASYNC] Exécution en tâche de fond...");
            super.send(message);
        }).start();
    }
}

public class Main {
    public static void main(String[] args) {
        Notifier notifier = new AsyncDecorator(
                new RetryDecorator(
                    new SMSDecorator(
                        new EmailDecorator(
                            new MessageNotifier()
                        )
                    ), 3, 1000
                )
             );
        notifier.send("Bonjour !");

    }
}
```

Chaque décorateur wrape le précédent et ajoute son comportement. Le `Main` n'a aucune idée de combien de couches il y a en dessous — il appelle juste `send()`.


### Aller plus loin : la Fluent API (Builder)

L'imbrication manuelle des décorateurs (approche "poupée russe") peut vite devenir illisible. On peut créer une **Fluent API** via un Builder pour cacher cette complexité :

```java
class NotifierBuilder {
    private Notifier notifier;

    private NotifierBuilder(Notifier base) {
        this.notifier = base;
    }

    public static NotifierBuilder create() {
        return new NotifierBuilder(new MessageNotifier());
    }

    public NotifierBuilder addEmail() {
        this.notifier = new EmailDecorator(this.notifier);
        return this;
    }

    public NotifierBuilder addSMS() {
        this.notifier = new SMSDecorator(this.notifier);
        return this;
    }

    public NotifierBuilder addLogging() {
        this.notifier = new LogDecorator(this.notifier);
        return this;
    }

    public NotifierBuilder withRetry(int attempts, long delay) {
        this.notifier = new RetryDecorator(this.notifier, attempts, delay);
        return this;
    }

    public NotifierBuilder async() {
        this.notifier = new AsyncDecorator(this.notifier);
        return this;
    }

    public Notifier build() {
        return this.notifier;
    }
}
```

Ce qui donne à l'usage :

```java
Notifier n = NotifierBuilder.create()
                .addEmail()
                .addSMS()
                .withRetry(3, 1000)
                .async()
                .build();

n.send("Succès total !");
```

L'avantage du Builder ici, c'est que l'auto-complétion de l'IDE montre directement les options disponibles, et les classes de décorateurs peuvent rester package-private — l'utilisateur n'a pas à les connaître.

## Avantages et inconvénients

**Avantages :**
- On peut combiner les comportements librement sans explosion de sous-classes
- Chaque décorateur a une seule responsabilité — facile à tester et à réutiliser
- On peut ajouter ou retirer des comportements à l'exécution
- Respecte le principe Open/Closed : on ajoute des décorateurs sans modifier les classes existantes

**Inconvénients :**
- L'imbrication manuelle (poupée russe) peut devenir illisible — d'où l'intérêt d'un Builder
- Beaucoup de petites classes si on a beaucoup de comportements à combiner
- L'ordre d'empilement des décorateurs peut avoir un impact sur le résultat (ex : le Log avant ou après le Retry ?)
- Difficulté à retirer un décorateur spécifique une fois la pile construite

## Sans ce pattern

Sans Decorator, on est obligé de créer une classe par combinaison de fonctionnalités :

```java
class EmailNotifier extends MessageNotifier { ... }
class SMSNotifier extends MessageNotifier { ... }
class LogNotifier extends MessageNotifier { ... }
class EmailAndSMSNotifier extends MessageNotifier { ... }
class EmailAndLogNotifier extends MessageNotifier { ... }
class SMSAndLogNotifier extends MessageNotifier { ... }
class EmailAndSMSAndLogNotifier extends MessageNotifier { ... }
// 3 options → 7 classes. 5 options → 31 classes.
```

Chaque nouvelle fonctionnalité double le nombre de classes. C'est l'explosion combinatoire : avec 5 comportements optionnels, on se retrouve avec 31 sous-classes à écrire et maintenir. Et si la logique d'envoi d'email change, il faut la modifier dans `EmailNotifier`, `EmailAndSMSNotifier`, `EmailAndLogNotifier`, `EmailAndSMSAndLogNotifier`…

Avec le Decorator, chaque comportement est une classe indépendante qu'on empile. 5 comportements = 5 classes, combinables à l'infini.

## Liens avec les autres concepts

**Patterns proches :**
- [Adapter](Adapter.md) — les deux wrappent un objet, mais le Decorator **garde la même interface** et ajoute du comportement, tandis que l'Adapter **change l'interface** pour la rendre compatible. Le Decorator empile, l'Adapter traduit.
- [Strategy](Strategy.md) — les deux permettent de varier le comportement, mais le Decorator **empile des couches** (email + SMS + log), tandis que le Strategy **substitue un algorithme entier** (voiture OU vélo). Le Decorator est additif, le Strategy est alternatif.
- [Facade](Facade.md) — la Facade simplifie un sous-système complexe. Le Builder du Decorator (Fluent API) joue un rôle similaire : il cache la complexité de l'empilement des décorateurs.

**Principes appliqués :**
- [Single Responsibility](../principles/Single%20Responsibility.md) — chaque décorateur n'a qu'**une seule** responsabilité (email, log, retry...). C'est ce qui rend la composition possible.
- [Open/Closed](../principles/Open%20&%20Closed.md) — on ajoute des comportements en créant de nouveaux décorateurs, sans modifier les classes existantes.
- [Loose Coupling](../principles/Loose%20Coupling.md) — chaque décorateur ne connaît que l'interface `Notifier`, pas les autres décorateurs de la pile.

**Différence clé avec le Strategy :** le Strategy dit "utilise cet algorithme **à la place** de celui-là". Le Decorator dit "ajoute ce comportement **en plus** de celui qui existe déjà". On peut combiner les deux : un Strategy pour choisir l'algorithme de base, et des Decorators pour y greffer du logging, du retry, etc.
