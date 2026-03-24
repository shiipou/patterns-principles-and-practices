# Practices : Code Reviewing

La code review, c'est quand un ou plusieurs développeurs relisent le code d'un collègue avant qu'il soit mergé. L'objectif n'est pas de chercher la petite bête, mais de détecter des bugs, d'améliorer la qualité du code, et de partager les connaissances dans l'équipe.

## Concept fondamental

La review sert quatre objectifs principaux :
1. **Détection d'erreurs** — identifier les bugs, défauts de logique ou problèmes de performance
2. **Qualité du code** — s'assurer que le code est bien structuré, lisible et conforme aux normes de l'équipe
3. **Partage de connaissances** — tout le monde reste au courant de ce qui se passe dans le projet
4. **Validation fonctionnelle** — vérifier que le code implémente correctement les spécifications

## Exemple

Le processus classique :
1. Un développeur soumet son code (pull request / merge request)
2. Un ou plusieurs pairs relisent ligne par ligne
3. Ils laissent des commentaires : bugs potentiels, suggestions d'amélioration, questions
4. Le développeur corrige et répond
5. Une fois les retours pris en compte, le code est approuvé et mergé

Ce qu'on cherche pendant une review :
- Des bugs ou des cas non gérés
- Du code difficile à comprendre ou à maintenir
- Des violations des conventions de l'équipe
- Des problèmes de performance ou de sécurité
- De la duplication qui pourrait être factorisée

Bonnes habitudes :
- Rester constructif : on critique le code, pas la personne
- Proposer des solutions plutôt que pointer des problèmes
- Faire des reviews régulièrement — ne pas laisser les PRs traîner
- Impliquer différents membres de l'équipe pour diversifier les points de vue

## Avantages et inconvénients

**Avantages :**
- Détection précoce des problèmes avant qu'ils ne deviennent coûteux
- Améliore la lisibilité, la cohérence et la robustesse du code
- Favorise l'apprentissage et le partage de connaissances dans l'équipe
- Renforce l'esprit d'équipe et la responsabilité collective

**Inconvénients :**
- Prend du temps, surtout si les PRs sont volumineuses
- Peut devenir un goulot d'étranglement si les reviews traînent
- Risque de frictions si les commentaires ne sont pas formulés avec tact
- Peut être superficiel si le reviewer manque de contexte

## Sans cette pratique

Sans code review, ce genre de bug passe directement en production :

```java
public void transferMoney(Account from, Account to, double amount) {
    from.setBalance(from.getBalance() - amount);
    to.setBalance(to.getBalance() + amount);
    // Pas de vérification de solde suffisant
    // Pas de transaction — si le serveur crash entre les deux lignes,
    // l'argent disparaît du premier compte sans arriver sur le second
}
```

Un reviewer aurait immédiatement pointé l'absence de vérification de solde et le manque de transaction. Sans review, un seul développeur décide de la qualité du code, et les mauvaises habitudes s'installent sans que personne ne les voie.

La code review n'est pas qu'un filet de sécurité : c'est aussi le moment où l'équipe partage ses connaissances et reste alignée sur les pratiques.
