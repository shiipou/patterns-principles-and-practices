# Principe : You Aren't Gonna Need It (YAGNI)

YAGNI dit simplement : n'implémente pas quelque chose tant que tu n'en as pas besoin *maintenant*. Pas "au cas où", pas "ça pourrait servir plus tard", pas "on va sûrement le demander".

## Concept fondamental

Le principe repose sur l'idée de rester concentré sur les exigences présentes et réelles du projet. Les besoins futurs qu'on imagine sont souvent faux — on passe du temps à coder quelque chose qui ne sera jamais utilisé, ou qui sera utilisé différemment de ce qu'on avait prévu.

YAGNI va de pair avec le développement itératif : on livre le minimum qui fonctionne, on récupère du feedback, puis on itère. Si demain on a vraiment besoin de cette fonctionnalité, on l'ajoutera à ce moment-là — avec une meilleure compréhension du besoin réel.

## Exemple

En pratique, ça veut dire :
- Ne pas développer une fonctionnalité qui n'est pas dans le sprint actuel
- Ne pas créer une abstraction pour un cas d'usage qui n'existe pas encore
- Ne pas prévoir 10 types de paiement quand le client n'en demande qu'un

Exemple concret : une équipe travaille sur un système de gestion de contenu et discute d'ajouter une fonctionnalité de recommandation de contenu personnalisé. Mais aucun utilisateur ne l'a demandée et aucune donnée d'utilisation ne la justifie. YAGNI dit : on ne la fait pas. Si elle devient nécessaire plus tard, on l'implémentera avec une vraie compréhension du besoin.

## Avantages et inconvénients

**Avantages :**
- Économie de temps et de ressources en se concentrant sur ce qui est réellement nécessaire
- Réduit la complexité du code et du système
- Flexibilité pour ajuster les priorités en fonction des retours utilisateurs

**Inconvénients :**
- Peut être utilisé comme excuse pour ne jamais anticiper ou architecturer
- Certains choix structurants doivent quand même être faits tôt (base de données, framework)
- Le refactoring tardif peut coûter cher si on a fait trop de compromis au départ

## Sans ce principe

Sans YAGNI, on code pour des besoins imaginaires :

```java
// Le client a demandé un export CSV. Rien d'autre.
interface DataExporter {
    void exportToCSV(Data data);
    void exportToXML(Data data);       // "au cas où"
    void exportToJSON(Data data);      // "ça pourrait servir"
    void exportToYAML(Data data);      // "on ne sait jamais"
    void exportToProtobuf(Data data);  // "c'est mieux d'anticiper"
    void exportToAvro(Data data);      // "tant qu'on y est"
}
```

5 méthodes sur 6 ne seront jamais utilisées. Mais elles doivent quand même être implémentées, testées et maintenues. Et le jour où on aura vraiment besoin d'un export JSON, le besoin réel sera probablement différent de ce qu'on a imaginé.

Avec YAGNI, on implémente l'export CSV. Point. Si le besoin d'un export JSON se confirme, on l'ajoutera à ce moment-là — avec une vraie compréhension du besoin.
