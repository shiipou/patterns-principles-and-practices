# Principe : Don't Repeat Yourself (DRY)

"Don't Repeat Yourself" (DRY) est un principe de conception logicielle qui encourage à éviter la duplication de code dans un système informatique. Le principe fondamental derrière DRY est de réduire la redondance en organisant le code de manière à ce que chaque morceau d'information ou de logique ne soit représenté qu'une seule fois.

## Concept Fondamental :

Le principe DRY stipule qu'une pièce spécifique d'information ou de logique dans un système logiciel devrait avoir une seule source d'autorité, ce qui signifie qu'elle ne devrait être écrite qu'une seule fois. Au lieu de copier-coller du code ou de dupliquer des informations, DRY encourage à factoriser le code répété en le plaçant dans des fonctions, des modules ou des composants réutilisables.

## Principes Clés :

1. **Élimination de la Redondance :** Évitez la duplication de code en factorisant les parties communes dans des fonctions ou des modules réutilisables.

2. **Clarté et Maintenabilité :** Le principe DRY favorise la clarté du code en assurant qu'un seul endroit définit chaque partie du système.

3. **Facilité de Maintenance :** En suivant DRY, les modifications ne doivent être apportées qu'à un seul endroit, ce qui simplifie la maintenance et réduit les risques d'erreurs.

## Exemple :

Imaginons un scénario où une application web utilise la valeur de la TVA (Taxes sur la Valeur Ajoutée) dans plusieurs endroits du code. Au lieu de répéter cette valeur à plusieurs endroits, on pourrait utiliser une constante ou une fonction pour représenter cette valeur unique :

```javascript
const VAT_RATE = 0.20; // Taux de TVA (20%)

function calculateTotalPrice(price) {
    return price * (1 + VAT_RATE); // Calcul du prix total avec TVA
}

// Utilisation de la fonction calculateTotalPrice
let productPrice = 100;
let totalPrice = calculateTotalPrice(productPrice);
console.log("Prix total avec TVA :", totalPrice);
```

Dans cet exemple, le taux de TVA est défini comme une constante `VAT_RATE`, ce qui évite de répéter la valeur `0.20` à chaque calcul de prix total. La fonction `calculateTotalPrice` encapsule la logique de calcul du prix total avec la TVA, de sorte que cette logique n'est écrite qu'une seule fois et peut être réutilisée à plusieurs endroits dans le code.

## Avantages de DRY :

- **Réduction de la Redondance :** Le code DRY réduit la duplication et favorise la réutilisation.
- **Clarté et Lisibilité :** Le code DRY est plus clair et plus facile à comprendre car il évite la redondance.
- **Maintenabilité Accrue :** Les modifications ne nécessitent des mises à jour que dans un seul endroit, ce qui simplifie la maintenance et réduit les risques d'erreurs.

## Conclusion :

En suivant le principe DRY, les développeurs peuvent améliorer la qualité, la clarté et la maintenabilité du code en évitant la duplication inutile. Cela permet également de faciliter l'évolution du système en introduisant des modifications de manière ciblée et cohérente.
