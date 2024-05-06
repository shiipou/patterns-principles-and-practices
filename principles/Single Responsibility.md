# Principe : Single Responsibility

Le principe de "Single Responsibility" (SRP), ou Principe de Responsabilité Unique, est un concept de conception logicielle qui encourage à ce qu'une classe ou un module ait une seule raison de changer. En d'autres termes, chaque élément de votre code (classe, fonction, module) doit avoir une seule responsabilité ou tâche à accomplir.

## Concept Fondamental :

Le principe de Responsabilité Unique stipule qu'un composant logiciel (comme une classe ou un module) doit avoir une seule raison de changer. Cela signifie qu'il ne devrait y avoir qu'une seule justification pour modifier ce composant. L'idée est de favoriser la modularité, la clarté et la maintenabilité du code en évitant les classes surchargées avec des responsabilités multiples.

## Principes Clés :

1. **Séparation des Responsabilités :** Chaque classe ou module doit se concentrer sur une seule tâche ou responsabilité.

2. **Cohésion Élevée :** Le SRP favorise la cohésion élevée en regroupant des fonctionnalités étroitement liées dans un même composant.

3. **Facilité de Maintenance :** En respectant le SRP, les modifications et les corrections de bugs deviennent plus ciblées et moins susceptibles d'entraîner des effets indésirables.

## Exemple :

Prenons l'exemple d'une application de gestion d'employés. Nous pourrions avoir une classe `Employee` qui représente un employé et qui contient des méthodes pour gérer les informations de l'employé (telles que le nom, le salaire, les informations de contact, etc.).

```java
public class Employee {
    private String name;
    private double salary;
    private String email;

    public Employee(String name, double salary, String email) {
        this.name = name;
        this.salary = salary;
        this.email = email;
    }

    public void updateSalary(double newSalary) {
        this.salary = newSalary;
        // Logique pour notifier l'employé ou effectuer d'autres actions liées à la mise à jour du salaire
    }

    public void sendEmail(String message) {
        // Logique pour envoyer un email à l'employé
    }

    // Méthodes pour gérer d'autres aspects de l'employé (non recommandées)

    // Méthode pour calculer les impôts sur le salaire de l'employé (responsabilité multiple)
    public double calculateTax() {
        // Logique pour calculer les impôts
        return this.salary * 0.2; // Exemple simplifié
    }
}
```

Dans cet exemple, la classe `Employee` a plusieurs responsabilités : gérer les informations de l'employé, mettre à jour le salaire, envoyer des e-mails, et même calculer les impôts sur le salaire. Cela viole le principe de Responsabilité Unique car la classe a plus d'une raison de changer : si les règles fiscales changent, si la logique d'envoi d'e-mails doit être mise à jour, etc.

Pour respecter le SRP, nous pourrions séparer les responsabilités en créant des classes distinctes :

- `Employee` : Responsable de la gestion des informations de base de l'employé.
- `SalaryCalculator` : Responsable du calcul des impôts sur le salaire.
- `EmailService` : Responsable de l'envoi d'e-mails.

## Avantages du SRP :

- **Meilleure Modularité :** Les classes modulaires sont plus faciles à comprendre, à tester et à maintenir.
- **Réutilisabilité :** Les classes avec une seule responsabilité sont plus réutilisables dans d'autres parties de l'application.
- **Moins de Couplage :** Les classes fortement couplées sont évitées, ce qui rend le système plus flexible et évolutif.

En conclusion, le principe de Responsabilité Unique est essentiel pour concevoir des systèmes logiciels modulaires et maintenables en réduisant la complexité et en favorisant une bonne séparation des responsabilités. Cela contribue à des applications plus robustes et évolutives sur le long terme.
