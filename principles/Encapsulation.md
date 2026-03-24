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
