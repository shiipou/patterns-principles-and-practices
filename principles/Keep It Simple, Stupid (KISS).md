# Principe : Keep It Simple, Stupid (KISS)

KISS rappelle une chose qu'on oublie vite : la simplicité est presque toujours préférable à la complexité. Si une solution simple fait le job, inutile de sortir l'artillerie lourde.

## Concept fondamental

Le principe repose sur l'idée que les systèmes simples sont plus faciles à comprendre, maintenir et étendre. Il encourage à éviter la sur-ingénierie : pas de design pattern si un `if` suffit, pas de microservices si un monolithe fait l'affaire, pas de couche d'abstraction juste "au cas où".

Ça ne veut pas dire qu'il ne faut jamais faire de l'architecture. Ça veut dire qu'il ne faut pas en faire quand ce n'est pas nécessaire.

## Exemple

En pratique, ça se traduit par :
- Une interface simple et ciblée sur les fonctionnalités essentielles
- Un modèle de données direct, sans couches inutiles
- Une architecture qui répond aux besoins actuels, pas à ceux qu'on imagine

Exemple typique : une équipe doit créer une appli de gestion de tâches pour une petite équipe. Au lieu de partir sur une architecture microservices avec un event bus et trois bases de données, on fait un monolithe propre avec une base relationnelle. Le jour où ça ne suffit plus, on itère.

## Avantages et inconvénients

**Avantages :**
- Le code simple est plus facile à lire, à tester et à débugger
- Moins de complexité = moins de bugs et moins d'endroits où un nouveau développeur peut se perdre
- Les solutions simples sont plus accessibles à tous les membres de l'équipe

**Inconvénients :**
- "Simple" est subjectif — ce qui est simple pour un développeur senior peut ne pas l'être pour un junior
- Peut être utilisé comme excuse pour ne pas faire d'architecture quand c'est réellement nécessaire
- Parfois, la solution simple d'aujourd'hui crée de la dette technique demain

## Sans ce principe

Sans KISS, un besoin simple devient une usine à gaz :

```java
// Valider qu'un email n'est pas vide...
interface ValidationRule<T> { boolean validate(T value); }
interface ValidationContext { Map<String, Object> getMetadata(); }
class ValidationPipeline<T> {
    private List<ValidationRule<T>> rules;
    private ValidationContext context;
    private ValidationResultAggregator aggregator;
    // ... 200 lignes
}

// Alors qu'il suffisait de :
if (email == null || email.isEmpty()) {
    throw new IllegalArgumentException("Email requis");
}
```

Un framework de validation générique avec pipeline, contexte et agrégateur… pour vérifier qu'un champ n'est pas vide. Le code est plus complexe à lire, à débugger et à maintenir que le problème qu'il est censé résoudre.

La solution simple fait le job. On complexifiera *si* et *quand* le besoin se présentera.

## Liens avec les autres concepts

**Principes proches :**
- [You Aren't Gonna Need It (YAGNI)](You%20Aren't%20Gonna%20Need%20It%20(YAGNI).md) — KISS dit "garde ça simple", YAGNI dit "ne fais pas ce qui n'est pas demandé". Les deux convergent : ne pas anticiper des besoins imaginaires, c'est garder le système simple.
- [Premature Optimization](Premature%20Optimization.md) — optimiser trop tôt viole KISS. On complexifie le code pour des gains non mesurés. KISS dit : écris du code clair d'abord, optimise ensuite si nécessaire.
- [Convention over configuration](Convention%20over%20configuration.md) — les conventions sont KISS appliqué à la configuration : plutôt que de configurer chaque détail, on suit une norme simple.

**Tensions avec d'autres concepts :**
- Tous les [design patterns](../patterns/) — chaque pattern ajoute de la complexité (interfaces, classes supplémentaires). KISS ne dit pas "jamais de pattern" mais "pas de pattern si un `if` suffit". Un [Strategy](../patterns/Strategy.md) pour 2 cas stables est du sur-engineering. Pour 10 cas qui évoluent, c'est justifié.
- [Abstraction](Abstraction.md) — trop d'abstraction tue la simplicité. 5 niveaux d'interfaces pour un seul cas d'usage concret, c'est une violation de KISS.
- [Open/Closed](Open%20&%20Closed.md) — rendre tout extensible "au cas où" ajoute de la complexité sans valeur immédiate.

**Pratiques liées :**
- [Solution Sketching](../practices/Solution%20Sketching.md) — dessiner avant de coder aide à trouver la solution la plus simple.
- [Code Reviewing](../practices/Code%20Reviewing.md) — un reviewer peut pointer qu'une solution est trop complexe pour le problème posé.
