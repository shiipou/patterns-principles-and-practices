# Principe : Single Responsibility

Une classe ne devrait avoir qu'une seule raison de changer. Si elle en a plusieurs, c'est qu'elle porte trop de casquettes.

## Concept fondamental

Le SRP dit qu'un composant logiciel (classe, fonction, module) ne doit avoir qu'une seule responsabilité. Une seule raison de changer. L'idée est de favoriser la modularité et la clarté en évitant les classes surchargées qui font trop de choses.

Le SRP ne veut pas dire "une méthode par classe". Une classe peut avoir plusieurs méthodes, tant qu'elles tournent toutes autour de la même responsabilité. Le SRP favorise aussi la haute cohésion : en regroupant des fonctionnalités étroitement liées dans un même composant.

## Exemple

Voici une classe `Employee` qui fait trop de choses :

```java
public class Employee {
    private String name;
    private double salary;
    private String email;

    public void updateSalary(double newSalary) { this.salary = newSalary; }
    public void sendEmail(String message) { /* envoi d'email */ }
    public double calculateTax() { return this.salary * 0.2; }
}
```

Cette classe a trois raisons de changer :
- Les règles de gestion des salaires évoluent
- La façon d'envoyer des emails change
- Le calcul d'impôts est modifié

Pour respecter le SRP, on sépare :
- `Employee` — les données de l'employé
- `TaxCalculator` — le calcul des impôts
- `EmailService` — l'envoi d'emails

Chaque classe a une seule responsabilité. Si les règles fiscales changent, seul `TaxCalculator` est impacté. Le reste ne bouge pas.

## Avantages et inconvénients

**Avantages :**
- Les classes modulaires sont plus faciles à comprendre, tester et maintenir
- Les classes avec une seule responsabilité sont plus réutilisables
- Moins de couplage entre les différentes préoccupations
- Les modifications sont plus ciblées et moins susceptibles de provoquer des effets de bord

**Inconvénients :**
- Peut mener à une explosion du nombre de classes si on pousse trop loin
- La frontière entre "une responsabilité" et "deux responsabilités" est parfois floue
- Plus de fichiers et plus de navigation dans le code

## Sans ce principe

Sans responsabilité unique, une seule classe fait tout :

```java
class UserService {
    public void register(String name, String email) {
        // Validation
        if (!email.contains("@")) throw new RuntimeException("Email invalide");

        // Persistance
        Connection conn = DriverManager.getConnection("jdbc:mysql://...");
        conn.prepareStatement("INSERT INTO users ...").executeUpdate();

        // Envoi d'email
        SmtpClient smtp = new SmtpClient("smtp.gmail.com");
        smtp.send(email, "Bienvenue !");

        // Log
        FileWriter fw = new FileWriter("app.log");
        fw.write("User registered: " + name);
    }
}
```

Changer de base de données ? Modifier `UserService`. Changer de service mail ? Modifier `UserService`. Changer le format de log ? Modifier `UserService`. Quatre raisons de changer = quatre chances de casser quelque chose à chaque modification.

Avec le SRP, chaque responsabilité est dans sa propre classe. Une modification n'impacte que la classe concernée.

## Liens avec les autres concepts

**Principes proches :**
- [Separation of Concerns](Separation%20of%20Concerns.md) — même idée, échelle différente. Le SRP s'applique à la **classe** ("une seule raison de changer"), la Separation of Concerns s'applique au **système** (séparer présentation, métier, données). Respecter le SRP dans chaque classe, c'est appliquer la Separation of Concerns au niveau micro.
- [High Cohesion](High%20Cohesion.md) — le SRP mène naturellement à la haute cohésion. Une classe avec une seule responsabilité a des éléments étroitement liés. La différence : le SRP regarde les **raisons de changer**, la cohésion regarde le **degré de relation** entre les éléments.
- [Don't Repeat Yourself (DRY)](Don't%20Repeat%20Yourself%20(DRY).md) — extraire une logique dupliquée dans sa propre classe, c'est appliquer DRY et SRP à la fois.
- [Loose Coupling](Loose%20Coupling.md) — des classes avec une seule responsabilité ont moins de raisons de dépendre d'autres classes. SRP réduit le couplage.

**Patterns qui appliquent ce principe :**
- [Decorator](../patterns/Decorator.md) — chaque décorateur n'a qu'**une seule** responsabilité (email, log, retry...). C'est ce qui rend la composition possible.
- [Strategy](../patterns/Strategy.md) — chaque stratégie a une seule responsabilité : implémenter un algorithme spécifique.
- [State](../patterns/State.md) — chaque classe d'état ne gère que le comportement propre à cet état.
- [Factory Method](../patterns/Factory%20Method.md) — sépare la logique de création de la logique d'utilisation.

**Pratiques liées :**
- [Code Reviewing](../practices/Code%20Reviewing.md) — les reviews sont le moment idéal pour détecter les classes qui portent trop de casquettes.
- [Naming](../practices/Naming.md) — si tu ne trouves pas un bon nom pour ta classe ("Manager", "Helper", "Utils"), c'est probablement qu'elle a trop de responsabilités.
