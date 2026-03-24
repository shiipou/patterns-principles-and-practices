# Principe : Convention over Configuration

L'idée est simple : au lieu de tout configurer à la main, on suit des conventions par défaut. Le framework prend des décisions automatiques tant qu'on respecte ses conventions. On ne configure que ce qui déroge à la norme.

## Concept fondamental

Les frameworks et outils définissent des conventions standard (nommage des fichiers, structure des répertoires, comportements par défaut). Tant que le développeur les respecte, tout fonctionne sans configuration explicite. On ne configure que ce qui sort de l'ordinaire. Ça réduit le bruit et permet de se concentrer sur la logique métier plutôt que sur la plomberie.

## Exemple

Ruby on Rails est le cas d'école. Si tu crées un modèle `User`, Rails s'attend à ce que la table en base s'appelle `users`, le contrôleur `UsersController`, et les vues soient dans `app/views/users/`. Tu n'as rien à configurer pour que ça marche.

Même logique avec Spring Boot en Java : tu poses un `application.properties` dans `src/main/resources/`, tu respectes la structure de packages, et le framework se débrouille. Pas de XML, pas de config manuelle.

Le principe s'applique aussi à plus petite échelle : dans un projet d'équipe, se mettre d'accord sur une arborescence de fichiers, un format de nommage ou un style de code, c'est déjà de la convention over configuration.

## Avantages et inconvénients

**Avantages :**
- Réduit la quantité de configuration nécessaire pour démarrer un projet
- Favorise l'uniformité et la cohérence du code entre développeurs
- Gain de temps : on se concentre sur le code qui a de la valeur

**Inconvénients :**
- Peut être contraignant si les conventions ne correspondent pas au besoin
- Courbe d'apprentissage : il faut connaître les conventions pour être productif
- Effet "magie noire" : quand tout est implicite, il est parfois difficile de comprendre ce qui se passe en coulisses

## Sans ce principe

Sans convention, on configure tout à la main :

```java
// Sans convention — configuration manuelle de chaque servlet
public class AppConfig {
    public void configure(ServletContext ctx) {
        ServletRegistration.Dynamic userServlet =
            ctx.addServlet("UserServlet", new UserServlet());
        userServlet.addMapping("/users");

        ServletRegistration.Dynamic orderServlet =
            ctx.addServlet("OrderServlet", new OrderServlet());
        orderServlet.addMapping("/orders");

        // ... répété pour chaque endpoint
    }
}

// Avec convention (Spring Boot) — le framework fait le mapping tout seul
@RestController
@RequestMapping("/users")
public class UserController {
    @GetMapping
    public List<User> getUsers() { /* ... */ }
}
```

50 lignes de configuration manuelle remplacées par deux annotations. Avec Spring Boot, le framework fait le reste tout seul.

Sans conventions, chaque développeur de l'équipe structure aussi son projet différemment. On perd du temps à chercher où sont les choses au lieu de coder.
