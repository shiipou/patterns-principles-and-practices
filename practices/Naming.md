# Practices : Naming

Bien nommer, c'est probablement la compétence la plus sous-estimée en développement. Un bon nom évite un commentaire, un mauvais nom crée de la confusion pour tous ceux qui liront le code après.

## Concept fondamental

Un bon nom est la meilleure documentation qui existe — et contrairement à un commentaire, il ne peut pas devenir obsolète tant que le code compile. Le but : que le code se lise comme une phrase. Si tu dois réfléchir plus de 2 secondes pour comprendre ce que fait une variable ou une fonction, c'est que son nom n'est pas assez clair.

Les règles de base :
- **Être descriptif :** `calculateTotal` plutôt que `calc`
- **Respecter les conventions du langage :** `getUserById` (camelCase en Java/JS), `get_user_by_id` (snake_case en Python)
- **Éviter les abréviations ambiguës :** `numberOfUsers` plutôt que `numOfUsrs`
- **Rester cohérent :** si on utilise `get` quelque part, on ne passe pas à `fetch` ailleurs pour la même chose

## Exemple

```java
// ❌ On comprend rien
public int calc(int[] arr) {
    int t = 0;
    for (int n : arr) { t += n; }
    return t;
}

// ✅ Immédiatement clair
public int calculateSum(int[] numbers) {
    int sum = 0;
    for (int number : numbers) { sum += number; }
    return sum;
}
```

Le deuxième se lit comme une phrase. Pas besoin de commentaire pour comprendre ce que ça fait.

## Avantages et inconvénients

**Avantages :**
- Le code est auto-documenté, réduisant le besoin de commentaires
- Facilite la lecture et la compréhension par tous les membres de l'équipe
- Réduit les malentendus et les erreurs d'interprétation

**Inconvénients :**
- Les noms descriptifs peuvent être longs (`calculateAverageUserSessionDuration`)
- Trouver le bon nom prend du temps et de la réflexion
- Les conventions varient entre langages et équipes, ce qui peut créer des frictions

## Sans cette pratique

Sans conventions de nommage, le code est indéchiffrable :

```java
class Mgr {
    private List<String> lst;
    public void proc(String s) {
        int x = Integer.parseInt(s);
        double r = x * 0.2;
        lst.add(String.valueOf(r));
    }
}
```

`Mgr` de quoi ? `lst` de quoi ? `proc` fait quoi ? `r` représente quoi ? 6 mois plus tard, même l'auteur ne sait plus ce que cette classe fait. Chaque développeur qui passe dessus doit lire tout le code pour reconstruire le contexte.

Avec de bons noms, le code se lit comme une phrase et les commentaires deviennent superflus.

## Liens avec les autres concepts

**Pratiques proches :**
- [Commenting](Commenting.md) — nommage et commentaires sont les deux faces de la même pièce. Un bon nom **remplace** un commentaire. `calculateSum(numbers)` n'a pas besoin de `// calcule la somme des nombres`. Le commentaire compense un mauvais nom, le bon nom élimine le besoin de commentaire.
- [Code Reviewing](Code%20Reviewing.md) — les noms mal choisis sautent aux yeux d'un reviewer. "Que signifie `mgr` ?" — la review force à choisir des noms clairs.

**Principes liés :**
- [Encapsulation](../principles/Encapsulation.md) — les noms des méthodes publiques révèlent l'**intention** (`withdraw`, `deposit`), pas l'implémentation. C'est l'encapsulation exprimée par le langage.
- [Single Responsibility](../principles/Single%20Responsibility.md) — si tu ne trouves pas un bon nom pour ta classe, c'est qu'elle fait trop de choses. Les noms vagues (`Manager`, `Helper`, `Utils`) sont un signal d'alarme : la classe a probablement trop de responsabilités.
- [Convention over configuration](../principles/Convention%20over%20configuration.md) — les conventions de nommage (`getUserById`, `isActive`, `OrderService`) sont un cas spécifique de ce principe. Si tout le monde nomme pareil, le code se comprend sans documentation.
- [Abstraction](../principles/Abstraction.md) — un bon nom abstrait les détails. `sendNotification()` plutôt que `openSmtpConnectionAndSendEmail()`. Le nom révèle le "quoi", pas le "comment".

**Patterns :**
- Les patterns eux-mêmes sont du naming : appeler une classe `FurnitureFactory` ou `NotifierDecorator` communique immédiatement son rôle à quelqu'un qui connaît les [design patterns](../patterns/).
