# Principe : Encapsulation

L'encapsulation est un concept clé en programmation orientée objet (POO) qui implique la restriction de l'accès aux données internes d'un objet et la protection de ces données en les cachant à l'extérieur. C'est l'un des principes fondamentaux de la POO visant à regrouper les données (variables) et les méthodes (fonctions) qui manipulent ces données au sein d'une même entité, appelée objet.

## Concept Fondamental :

L'encapsulation consiste à encapsuler (cacher) les détails internes d'un objet et à exposer uniquement une interface (méthodes publiques) permettant d'interagir avec cet objet. En d'autres termes, les détails internes (comme les variables d'instance) ne sont pas accessibles directement depuis l'extérieur de l'objet, mais uniquement à travers des méthodes spécifiques.

## Principes Clés :

1. **Protection des Données :** Les données internes d'un objet sont cachées et ne peuvent être modifiées qu'à travers des méthodes spécifiques, ce qui permet de protéger l'intégrité des données.

2. **Interface Publique :** Seules les méthodes publiques (interface) sont accessibles depuis l'extérieur de l'objet, offrant une abstraction des détails internes et permettant de contrôler l'accès aux fonctionnalités de l'objet.

3. **Modularité et Réutilisabilité :** L'encapsulation favorise la modularité en regroupant étroitement les données et les opérations qui les manipulent, ce qui facilite la réutilisation du code.

## Exemple :

Voici un exemple simple en Java illustrant l'encapsulation :

```java
public class Person {
    private String name;
    private int age;

    // Constructeur
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Méthodes publiques pour accéder et modifier les données encapsulées
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        } else {
            System.out.println("L'âge doit être un nombre positif.");
        }
    }
}
```

Dans cet exemple, la classe `Person` encapsule les données `name` et `age` en les déclarant comme privées (utilisation de l'encapsulation) et en fournissant des méthodes publiques (`getName`, `getAge`, `setAge`) pour accéder et modifier ces données de manière contrôlée.

L'encapsulation permet ici de protéger les données `name` et `age` en empêchant un accès direct depuis l'extérieur de la classe. Au lieu de cela, l'accès à ces données est contrôlé à travers des méthodes publiques, ce qui garantit que les règles de validation (comme vérifier si l'âge est positif dans `setAge`) sont respectées.

## Avantages de l'Encapsulation :

- **Protection des Données :** Les données sensibles sont cachées et ne peuvent être manipulées qu'à travers des méthodes spécifiques.
- **Contrôle d'Accès :** L'encapsulation permet de définir des règles d'accès pour manipuler les données, améliorant ainsi la sécurité et la fiabilité du code.
- **Abstraction :** Fournit une abstraction des détails internes, ce qui facilite la maintenance, la réutilisabilité et la collaboration dans le développement logiciel.

En conclusion, l'encapsulation est un principe important de la POO qui contribue à la création de systèmes logiciels modulaires, sécurisés et robustes en cachant les détails internes et en exposant uniquement une interface contrôlée pour interagir avec les objets.
