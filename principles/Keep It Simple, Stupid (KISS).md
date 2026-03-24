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
