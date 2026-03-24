# Practices : Performance Optimizing

Optimiser les performances, c'est rendre une application plus rapide ou moins gourmande en ressources. Mais la règle numéro un, c'est : **mesurer avant d'optimiser**.

## Concept fondamental

L'optimisation des performances suit une démarche itérative et basée sur des mesures :
1. **Identifier le problème** — l'appli est lente, mais où exactement ? Utiliser un profiler (VisualVM, Chrome DevTools, `perf`) pour trouver les vrais goulets d'étranglement.
2. **Cibler** — optimiser le code qui prend 80% du temps, pas celui qui prend 0.1%.
3. **Corriger** — améliorer l'algo, ajouter un cache, optimiser une requête SQL, paralléliser un traitement.
4. **Mesurer à nouveau** — vérifier que le changement a vraiment amélioré les choses.

Il faut toujours garder un équilibre entre performance et lisibilité du code. Un code illisible mais rapide est un code qu'on ne pourra plus maintenir.

## Exemple

Les leviers courants :
- **Algorithmique** — passer d'un O(n²) à un O(n log n) peut transformer une opération de 10 secondes en 10 millisecondes
- **Base de données** — ajouter un index, optimiser une requête, éviter le N+1
- **Cache** — ne pas recalculer ou re-fetcher ce qui ne change pas souvent
- **Parallélisme** — répartir le travail sur plusieurs threads ou processus
- **Lazy loading** — ne charger que ce dont on a besoin, quand on en a besoin
- **Gestion mémoire** — minimiser les fuites de mémoire et les allocations excessives

Exemple concret : une appli web met 2 secondes à charger. Après profilage, on identifie des requêtes SQL inefficaces. On ajoute des index et on réduit les appels inutiles à la base — le temps de chargement passe à 200ms.

## Avantages et inconvénients

**Avantages :**
- Améliore l'expérience utilisateur (temps de réponse, fluidité)
- Réduit la consommation de ressources (CPU, mémoire, réseau)
- Permet de supporter des charges de travail croissantes

**Inconvénients :**
- Peut rendre le code plus complexe et moins lisible
- Risque d'optimiser au mauvais endroit sans mesures préalables
- Les optimisations peuvent créer de nouveaux bugs ou effets de bord

## Sans cette pratique

Sans optimisation, le code fait des choses absurdes sans que personne ne s'en rende compte :

```java
// Problème N+1 : 1 requête pour les commandes, puis 1 requête par commande
public List<OrderDTO> getOrders() {
    List<Order> orders = db.query("SELECT * FROM orders"); // 1 requête
    for (Order order : orders) {
        User user = db.query(
            "SELECT * FROM users WHERE id = ?", order.getUserId() // N requêtes
        );
        order.setUser(user);
    }
    return orders;
}
// 1000 commandes = 1001 requêtes SQL.
// Un simple JOIN réduirait ça à 1 seule requête.
```

La page met 5 secondes à charger et personne ne sait pourquoi. Un profiler aurait montré immédiatement que 99% du temps est passé dans les requêtes SQL.

Mesurer avant d'optimiser — et optimiser au bon endroit.
