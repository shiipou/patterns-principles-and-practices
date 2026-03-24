# Design Pattern : Singleton

Le Singleton garantit qu'une classe n'a qu'une seule instance dans toute l'application, et fournit un accès global à cette instance. Le constructeur est privé : impossible d'instancier la classe de l'extérieur.

## Concept fondamental

Le constructeur de la classe est déclaré `private`, ce qui empêche toute instanciation depuis l'extérieur. Une méthode statique `getInstance()` vérifie si une instance existe déjà : si oui, elle la retourne ; sinon, elle en crée une. Résultat : peu importe combien de fois on appelle `getInstance()`, on obtient toujours le même objet.

## Exemple

Un cas classique : un gestionnaire de configuration qu'on veut unique dans toute l'appli.

```java
public class AppConfig {
    private static AppConfig instance;
    private String databaseUrl;

    // Constructeur privé = pas de new AppConfig() depuis l'extérieur
    private AppConfig() {
        this.databaseUrl = "jdbc:postgresql://localhost:5432/mydb";
    }

    public static AppConfig getInstance() {
        if (instance == null) {
            instance = new AppConfig();
        }
        return instance;
    }

    public String getDatabaseUrl() {
        return databaseUrl;
    }
}

public class Main {
    public static void main(String[] args) {
        AppConfig config = AppConfig.getInstance();
        System.out.println(config.getDatabaseUrl());

        // Même instance partout
        AppConfig config2 = AppConfig.getInstance();
        System.out.println(config == config2); // true
    }
}
```

`AppConfig.getInstance()` retourne toujours la même instance. Le constructeur privé empêche toute instanciation directe.

⚠️ Attention : cette version simple n'est pas thread-safe. En multi-thread, deux appels simultanés à `getInstance()` pourraient créer deux instances. Pour y remédier, on peut synchroniser la méthode ou utiliser le pattern "holder" avec une classe interne.

## Avantages et inconvénients

**Avantages :**
- Garantit une instance unique partagée dans toute l'application
- Accès global simple via `getInstance()`
- L'instance n'est créée que si elle est vraiment utilisée (lazy initialization)

**Inconvénients :**
- Rend le code plus difficile à tester (difficulté à mocker l'instance)
- Introduit un état global, ce qui peut masquer des dépendances entre classes
- Problèmes de thread-safety si l'implémentation n'est pas soignée
- Souvent considéré comme un anti-pattern quand il est utilisé abusivement

## Sans ce pattern

Sans Singleton, rien n'empêche de créer plusieurs instances là où une seule devrait exister :

```java
// Dans le module A
AppConfig configA = new AppConfig("config.yml");
configA.set("debug", true);

// Dans le module B
AppConfig configB = new AppConfig("config.yml");
System.out.println(configB.get("debug")); // false — incohérent !
```

Deux instances lisent le même fichier mais ne partagent pas leur état en mémoire. L'une active le mode debug, l'autre ne le voit pas. Le même problème se pose pour un pool de connexions (on crée plusieurs pools au lieu d'un seul) ou un logger (les logs partent dans des fichiers différents).

Avec le Singleton, `AppConfig.getInstance()` retourne toujours la même instance. Tout le monde partage le même état.
