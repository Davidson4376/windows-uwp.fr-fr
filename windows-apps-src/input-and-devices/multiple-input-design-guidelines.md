---
author: Karl-Bridge-Microsoft
Description: Tout comme les personnes ont recours à une combinaison de voix et de mouvement pour communiquer entre elles, plusieurs types et modes d’entrée peuvent également être utilisés lors de l’interaction avec une application.
title: Recommandations en matière de conception d’entrées multiples
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
---

# Entrées multiples

Tout comme les personnes ont recours à une combinaison de voix et de mouvement pour communiquer entre elles, plusieurs types et modes d’entrée peuvent également être utilisés lors de l’interaction avec une application.


Pour vous adapter au plus grand nombre possible d’utilisateurs et d’appareils, nous vous recommandons de concevoir vos applications de manière à ce qu’elles fonctionnent avec le plus grand nombre possible de types d’entrées (mouvement, commande vocale, écran tactile, pavé tactile, souris et clavier). Cela a pour effet d’optimiser la flexibilité, la facilité d’utilisation et l’accessibilité.

Pour commencer, envisagez les différentes situations dans lesquelles votre application gère des entrées. Essayez d’être cohérent dans l’ensemble de votre application et n’oubliez pas que les contrôles de plateforme offrent une prise en charge intégrée de plusieurs types d’entrée.

-   Les utilisateurs peuvent interagir avec l’application à l’aide de plusieurs périphériques d’entrée ?
-   Toutes les méthodes d’entrée sont-elles prises en charge à tout moment ? Avec certains contrôles ? À des moments ou dans des circonstances spécifiques ?
-   Une méthode d’entrée a-t-elle la priorité ?

## <span id="Single__or_exclusive_-mode_interactions_"></span><span id="single__or_exclusive_-mode_interactions_"></span><span id="SINGLE__OR_EXCLUSIVE_-MODE_INTERACTIONS_"></span>Interactions en mode unique (ou exclusif)


Avec les interactions en mode unique, plusieurs types d’entrée sont pris en charge, mais un seul peut être utilisé par action. Par exemple, la reconnaissance vocale pour les commandes, et les mouvements pour la navigation ; ou bien la saisie de texte par interface tactile ou à l’aide de mouvements, en fonction de la proximité.

## <span id="Multimodal_interactions"></span><span id="multimodal_interactions"></span><span id="MULTIMODAL_INTERACTIONS"></span>Interactions multimodales


Avec les interactions multimodales, plusieurs méthodes d’entrée séquentielles sont utilisées pour effectuer une action unique.

<span id="Speech___gesture"></span><span id="speech___gesture"></span><span id="SPEECH___GESTURE"></span>Reconnaissance vocale + mouvement  
L’utilisateur désigne un produit, puis dit « Ajouter au panier ».

<span id="Speech___touch"></span><span id="speech___touch"></span><span id="SPEECH___TOUCH"></span>Reconnaissance vocale + interface tactile  
L’utilisateur sélectionne une photo en appuyant de façon prolongée dessus, puis dit « Envoyer photo ».





<!--HONumber=May16_HO2-->


