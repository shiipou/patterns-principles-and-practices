# Principe : Abstraction

L'abstraction, c'est le fait de ne garder que l'essentiel d'un concept en cachant les détails d'implémentation. On modélise "ce que ça fait" sans se soucier de "comment ça le fait".

## Concept fondamental

Abstraire, c'est choisir ce qu'on expose et ce qu'on cache. En Java, ça passe par les interfaces (qui définissent un contrat sans implémentation) et les classes abstraites (qui fixent certains comportements et en laissent d'autres aux sous-classes). L'abstraction permet de se concentrer sur les aspects importants d'un problème sans se noyer dans les détails techniques.

Elle s'applique aussi à la modélisation : quand on représente un objet du monde réel dans le code, on ne garde que les propriétés utiles pour le système.

## Exemple

```java
// L'interface dit "ce que ça fait" : calculer une aire et un périmètre
public interface Shape {
    double getArea();
    double getPerimeter();
}

// La classe abstraite peut fixer certains comportements
// et en laisser d'autres aux sous-classes
public abstract class Animal {
    public abstract void makeSound();

    public void eat() {
        System.out.println("L'animal mange.");
    }
}
```

On peut aussi parler d'abstraction quand on modélise un objet du monde réel en ne gardant que les propriétés utiles :

```java
public class Book {
    private String title;
    private String author;
    private int numberOfPages;
}
```

Un vrai livre a une couverture, un poids, une odeur… Mais dans notre système de bibliothèque, seuls le titre, l'auteur et le nombre de pages nous intéressent. C'est ça l'abstraction : choisir ce qu'on garde et ce qu'on ignore.

## Avantages et inconvénients

**Avantages :**
- Simplifie la compréhension du code en masquant les détails non pertinents
- Favorise la modularité et la réutilisabilité des composants
- Facilite l'extension et l'évolution du système sans impacter les autres parties du code

**Inconvénients :**
- Trop d'abstraction rend le code difficile à suivre ("où est l'implémentation concrète ?")
- Peut mener à des architectures sur-ingéniériées si on abstrait des choses qui ne changeront jamais
- Nécessite de bien choisir le bon niveau d'abstraction — trop haut ou trop bas pose problème

## Sans ce principe

Sans abstraction, le code est collé aux implémentations concrètes :

```java
class ReportService {
    public void generate(MySQLDatabase db) {
        ResultSet rs = db.executeQuery("SELECT * FROM sales");
        // ... traitement lié à MySQL
    }
}
```

Le jour où on veut passer à PostgreSQL, il faut modifier `ReportService` — alors que la logique de génération du rapport n'a rien à voir avec le choix de la base de données.

Avec une abstraction (`Database` ou `DataSource`), `ReportService` ne connaît que l'interface. On change de base sans toucher au rapport.

## Liens avec les autres concepts

**Principes proches :**
- [Encapsulation](Encapsulation.md) — complémentaires mais différents. L'abstraction définit **ce qu'on expose** (l'interface publique), l'encapsulation définit **ce qu'on cache** (les détails internes). L'abstraction choisit le bon niveau de détail, l'encapsulation protège les données derrière ce niveau.
- [Dependency Inversion](Dependency%20Inversion.md) — le Dependency Inversion dit "dépends d'abstractions, pas de concrétions". L'abstraction est le mécanisme qui rend ça possible : sans interfaces ni classes abstraites, pas d'inversion de dépendance.
- [Loose Coupling](Loose%20Coupling.md) — l'abstraction est le principal levier pour obtenir un couplage lâche. En dépendant d'une interface plutôt que d'une classe concrète, on découple les modules.
- [Open/Closed](Open%20&%20Closed.md) — les abstractions (interfaces) permettent d'étendre le système sans modifier le code existant.

**Patterns qui s'appuient sur l'abstraction :**
- [Strategy](../patterns/Strategy.md), [Observer](../patterns/Observer.md), [Factory Method](../patterns/Factory%20Method.md), [Abstract Factory](../patterns/Abstract%20Factory.md), [Adapter](../patterns/Adapter.md) — tous ces patterns reposent sur des interfaces pour découpler le code client des implémentations concrètes.

**Point de vigilance :** trop d'abstraction nuit à la lisibilité. Si tu dois naviguer 5 interfaces pour comprendre ce que fait un appel, c'est qu'on a trop abstrait. Le principe [KISS](Keep%20It%20Simple,%20Stupid%20(KISS).md) rappelle qu'il faut abstraire uniquement quand c'est justifié.
