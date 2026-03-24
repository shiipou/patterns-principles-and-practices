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
