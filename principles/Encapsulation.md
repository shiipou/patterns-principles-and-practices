# Principe : Encapsulation

L'encapsulation consiste à cacher les données internes d'un objet et à ne les rendre accessibles qu'à travers des méthodes contrôlées. De l'extérieur, on ne voit que l'interface publique — les détails internes restent privés.

## Concept fondamental

L'idée est de regrouper les données (variables) et les méthodes qui les manipulent au sein d'un même objet, tout en contrôlant ce qui est visible de l'extérieur. Les données internes sont déclarées `private`, et on expose uniquement des méthodes publiques (getters, setters) qui permettent d'y accéder de façon contrôlée.

Ça permet deux choses :
1. **Protéger l'intégrité des données** — on peut valider les valeurs avant de les accepter
2. **Masquer les détails d'implémentation** — on peut changer la structure interne sans casser le code qui utilise l'objet

## Exemple

```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }

    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        } else {
            System.out.println("L'âge doit être positif.");
        }
    }
}
```

`name` et `age` sont privés. On ne peut pas faire `person.age = -5` directement — il faut passer par `setAge()`, qui vérifie que la valeur est valide.

## Avantages et inconvénients

**Avantages :**
- Protège les données sensibles en empêchant un accès direct
- Permet de définir des règles de validation sur les modifications
- Fournit une abstraction des détails internes, ce qui facilite la maintenance et la réutilisabilité
- Réduit le couplage : le code extérieur ne dépend que de l'interface publique

**Inconvénients :**
- Peut alourdir le code avec beaucoup de getters/setters (surtout si on ne fait que passer les valeurs sans logique)
- Un excès d'encapsulation peut rendre le code plus verbeux sans apporter de valeur réelle
- Nécessite de bien réfléchir à ce qui doit être public et ce qui doit rester privé

## Sans ce principe

Sans encapsulation, n'importe qui peut mettre n'importe quoi :

```java
class BankAccount {
    public double balance;
    public String owner;
}

BankAccount account = new BankAccount();
account.balance = -5000;  // Solde négatif — rien ne l'empêche
account.owner = "";       // Propriétaire vide — pas de validation
```

N'importe quel bout de code peut mettre le solde à -5000 ou vider le nom du propriétaire. Il n'y a aucune règle métier, aucun contrôle. Le jour où on veut ajouter une validation, il faut retrouver tous les endroits qui modifient `balance` directement.

Avec l'encapsulation, `balance` est privé et passe par `withdraw()` / `deposit()` qui vérifient les règles. Impossible de contourner la validation.

## Liens avec les autres concepts

**Principes proches :**
- [Abstraction](Abstraction.md) — complémentaires. L'abstraction définit **ce qu'on expose** (le contrat public), l'encapsulation définit **ce qu'on protège** (les données internes). L'abstraction choisit le bon niveau de détail, l'encapsulation empêche le contournement de ce niveau.
- [Single Responsibility](Single%20Responsibility.md) — un objet bien encapsulé a naturellement une responsabilité claire : il gère ses propres données et les règles associées.
- [High Cohesion](High%20Cohesion.md) — l'encapsulation regroupe les données et les méthodes qui les manipulent. C'est la cohésion à l'échelle de la classe.
- [Loose Coupling](Loose%20Coupling.md) — en cachant les détails internes, l'encapsulation réduit le couplage : le code extérieur ne dépend que de l'interface publique, pas de la structure interne.

**Patterns qui s'appuient sur l'encapsulation :**
- [State](../patterns/State.md) — chaque état encapsule son propre comportement et ses transitions. Le contexte ne voit que l'interface `State`.
- [Facade](../patterns/Facade.md) — la Facade encapsule les interactions complexes entre sous-systèmes derrière une API simple.
- [Decorator](../patterns/Decorator.md) — chaque décorateur encapsule un comportement supplémentaire sans exposer les détails des couches en dessous.

**Pratiques liées :**
- [Naming](../practices/Naming.md) — les noms des méthodes publiques (`withdraw`, `deposit`) révèlent l'**intention**, pas l'implémentation. C'est l'encapsulation au niveau du langage.
