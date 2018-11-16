---
author: Karl-Bridge-Microsoft
Description: Just as people use a combination of voice and gesture when communicating with each other, multiple types and modes of input can also be useful when interacting with an app.
title: Recommandations en matière de conception d’entrées multiples
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 670caf5c519fcf1b7ff57b47781541d98358aa50
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6836010"
---
# <a name="multiple-inputs"></a>Entrées multiples


Tout comme les personnes ont recours à une combinaison de voix et de mouvement pour communiquer entre elles, plusieurs types et modes d’entrée peuvent également être utilisés lors de l’interaction avec une application.


Pour vous adapter au plus grand nombre possible d’utilisateurs et d’appareils, nous vous recommandons de concevoir vos applications de manière à ce qu’elles fonctionnent avec le plus grand nombre possible de types d’entrées (mouvement, commande vocale, écran tactile, pavé tactile, souris et clavier). Cela a pour effet d’optimiser la flexibilité, la facilité d’utilisation et l’accessibilité.

Pour commencer, envisagez les différentes situations dans lesquelles votre application gère des entrées. Essayez d’être cohérent dans l’ensemble de votre application et n’oubliez pas que les contrôles de plateforme offrent une prise en charge intégrée de plusieurs types d’entrée.

-   Les utilisateurs peuvent interagir avec l’application à l’aide de plusieurs périphériques d’entrée ?
-   Toutes les méthodes d’entrée sont-elles prises en charge à tout moment ? Avec certains contrôles ? À des moments ou dans des circonstances spécifiques ?
-   Une méthode d’entrée a-t-elle la priorité?

## <a name="single-or-exclusive-mode-interactions"></a>Interactions en mode unique (ou exclusif)


Avec les interactions en mode unique, plusieurs types d’entrée sont pris en charge, mais un seul peut être utilisé par action. Par exemple, la reconnaissance vocale pour les commandes, et les mouvements pour la navigation ; ou bien la saisie de texte par interface tactile ou à l’aide de mouvements, en fonction de la proximité.

## <a name="multimodal-interactions"></a>Interactions multimodales

Avec les interactions multimodales, plusieurs méthodes d’entrée séquentielles sont utilisées pour effectuer une action unique.

Reconnaissance vocale + mouvement  
L’utilisateur désigne un produit, puis dit « Ajouter au panier ».

Reconnaissance vocale + interface tactile  
L’utilisateur sélectionne une photo en appuyant de façon prolongée dessus, puis dit «Envoyer photo».



