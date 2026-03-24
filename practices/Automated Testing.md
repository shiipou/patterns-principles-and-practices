# Practices : Automated Testing

Les tests automatisés, c'est écrire du code qui vérifie que le code fonctionne. Au lieu de tester manuellement à chaque changement, on lance une suite de tests qui valide tout en quelques secondes.

## Concept fondamental

L'idée est de détecter les erreurs le plus tôt possible, avant le déploiement en production. Chaque test vérifie une chose précise, de façon indépendante et reproductible. On intègre les tests dans la CI/CD : à chaque push, les tests tournent automatiquement. Si un test échoue, le merge est bloqué.

Les différents types de tests forment une pyramide :
- **Tests unitaires** — testent une fonction ou une classe en isolation. Rapides, nombreux, exécutés à chaque commit. (JUnit, pytest, NUnit)
- **Tests d'intégration** — vérifient que plusieurs composants fonctionnent ensemble (API + base de données par ex.)
- **Tests de régression** — repassent les anciens tests pour s'assurer qu'un nouveau changement n'a rien cassé
- **Tests de charge** — mesurent comment l'appli se comporte sous stress (JMeter, Locust)
- **Tests end-to-end** — simulent le parcours utilisateur complet dans un navigateur (Selenium, Cypress)

## Exemple

Un test doit être indépendant : s'il passe seul, il doit passer dans la suite complète. Chaque test ne vérifie qu'une chose. Si un test échoue, on doit comprendre immédiatement ce qui a cassé.

Bonnes pratiques :
- Développer des tests indépendants les uns des autres
- Organiser les tests en suites modulaires
- Écrire des tests lisibles et clairs pour faciliter la collaboration
- Exécuter les tests régulièrement dans les workflows d'intégration continue

Outils utiles : JUnit/pytest/NUnit pour les tests unitaires, Selenium/Cypress pour les tests UI, JMeter/Locust pour la charge, Postman/REST Assured pour les API.

## Avantages et inconvénients

**Avantages :**
- Détection précoce des bugs avant le déploiement
- Tests reproductibles et rapides comparés aux tests manuels
- Couverture étendue : on peut couvrir un large éventail de scénarios
- S'intègrent dans la CI/CD pour des déploiements continus et fiables

**Inconvénients :**
- Coût initial d'écriture et de maintenance des tests
- Les tests fragiles (flaky tests) peuvent faire perdre confiance dans la suite de tests
- Ne remplacent pas complètement les tests exploratoires manuels
- Un faux sentiment de sécurité si la couverture est bonne mais les scénarios mal choisis

## Sans cette pratique

Sans tests, un bug silencieux passe en production :

```java
public double calculateDiscount(double total, boolean isPremium) {
    if (isPremium) {
        return total * 0.15;
    }
    if (total > 100) {
        return total * 0.05;
    }
    return 0;
}
```

Ce code a l'air correct. Mais personne n'a testé : que se passe-t-il si `total` est négatif ? Un client premium avec un total > 100 cumule-t-il les deux réductions ? Qu'arrive-t-il si `total` vaut exactement 100 ?

Sans test automatisé, ces questions restent sans réponse jusqu'à ce qu'un client se plaigne. Et quand quelqu'un modifie cette méthode dans 6 mois, rien ne vérifie que les cas existants fonctionnent toujours.

Avec un test unitaire, chaque cas est vérifié automatiquement à chaque commit. Une régression est détectée en secondes, pas en production.
