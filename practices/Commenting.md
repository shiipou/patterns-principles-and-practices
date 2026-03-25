# Practices : Commenting

Un bon commentaire explique **pourquoi**, pas **quoi**. Le code dit déjà ce qu'il fait — un commentaire doit expliquer l'intention, le contexte ou les pièges que le code seul ne peut pas transmettre.

## Concept fondamental

Les commentaires servent à combler l'écart entre ce que le code *fait* et ce que le développeur *voulait* faire. Si le code est suffisamment expressif (bon nommage, structure claire), la plupart des commentaires deviennent inutiles. La règle d'or : si tu sens le besoin de commenter un morceau de code, demande-toi d'abord si tu ne pourrais pas le rendre plus clair en le renommant ou en le restructurant.

## Exemple

**Commentaires inline** — à côté d'une ligne pour clarifier un point précis :
```java
// On trie par date décroissante pour afficher les plus récents en premier
results.sort(Comparator.comparing(Result::getDate).reversed());
```

**Commentaires de documentation** — pour les fonctions et classes publiques :
```java
/**
 * Calcule la somme d'une liste de nombres.
 *
 * @param numbers la liste de nombres à additionner
 * @return la somme de tous les éléments
 */
public double calculateTotal(List<Double> numbers) {
    return numbers.stream().mapToDouble(Double::doubleValue).sum();
}
```

**Commentaires de bloc** — pour expliquer un algorithme ou une section complexe.

Un commentaire ne doit **jamais servir de pansement** pour du mauvais code. Si tu te retrouves à écrire un paragraphe pour expliquer ce que fait un bloc, c'est que le code est trop compliqué — il faut le refactorer, pas le commenter :

```java
// ❌ Le commentaire compense du code illisible
// Vérifie si l'utilisateur est majeur, actif et non banni
if (u.getA() >= 18 && u.getS() == 1 && !u.getB()) { ... }

// ✅ Le code est clair, le commentaire est inutile
if (user.isAdult() && user.isActive() && !user.isBanned()) { ... }
```

Les erreurs courantes :
- Commenter l'évident : `i += 1  // incrémente i` → inutile
- Laisser des commentaires obsolètes qui ne correspondent plus au code
- Commenter du code mort au lieu de le supprimer (le VCS est là pour ça)
- **Utiliser les commentaires pour justifier du code confus** — rends le code lisible à la place

## Avantages et inconvénients

**Avantages :**
- Explique le "pourquoi" derrière des choix techniques non évidents
- Facilite la compréhension pour les nouveaux développeurs
- La documentation de fonctions publiques améliore l'utilisation de l'API

**Inconvénients :**
- Les commentaires peuvent devenir obsolètes et mentir sur ce que fait le code
- Trop de commentaires encombrent le code et réduisent la lisibilité
- Commenter un code mal écrit est un pansement — mieux vaut le rendre plus clair

## Sans cette pratique

Sans bonnes pratiques de commentaires, le code ment :

```java
// ❌ Commentaire mensonger (le code a changé, pas le commentaire)
// Retourne le prix HT
public double getPrice() {
    return price * 1.20; // En fait c'est le TTC...
}

// ❌ Commentaire inutile
i++; // On incrémente i

// ✅ Commentaire utile : explique le POURQUOI
// On attend 300ms car l'API de paiement renvoie un 429
// si on enchaîne les requêtes trop vite (cf. ticket JIRA-4521)
Thread.sleep(300);
```

Un commentaire obsolète est pire que pas de commentaire — il induit en erreur. Un commentaire qui répète le code est du bruit. Le bon commentaire explique ce que le code ne peut pas dire : l'intention, le contexte, le piège.

## Liens avec les autres concepts

**Pratiques proches :**
- [Naming](Naming.md) — complémentaires et souvent opposés. Un bon nommage **élimine** le besoin de commentaires. Si `calculateSum(numbers)` est clair, le commenter est inutile. Si tu commentes beaucoup, c'est peut-être que tes noms ne sont pas assez expressifs.
- [Code Reviewing](Code%20Reviewing.md) — la review est le moment idéal pour détecter les commentaires inutiles, mensongers ou manquants. "Ce commentaire n'est plus à jour" est un feedback de review classique.

**Principes liés :**
- [KISS](../principles/Keep%20It%20Simple,%20Stupid%20(KISS).md) — un code simple n'a pas besoin de commentaires. Si tu dois écrire un paragraphe pour expliquer ce que fait un bloc, c'est que le code est trop compliqué — simplifie le code, pas le commentaire.
- [Abstraction](../principles/Abstraction.md) — une bonne abstraction révèle l'intention sans qu'on ait besoin de commenter. `paymentGateway.charge(amount)` n'a pas besoin de commentaire.

**Le piège du "commentaire pansement" :**
Si tu te retrouves à commenter du code pour le rendre compréhensible, applique plutôt [Naming](Naming.md) (meilleurs noms), [Single Responsibility](../principles/Single%20Responsibility.md) (découper la classe) ou [Abstraction](../principles/Abstraction.md) (extraire une méthode bien nommée). Le commentaire doit être le **dernier recours**, pas le premier réflexe.
