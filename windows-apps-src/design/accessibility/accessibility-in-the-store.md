---
Description: Describes the requirements for declaring your Universal Windows Platform (UWP) app as accessible in the Microsoft Store.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Accessibilité dans le Store
label: Accessibility in the Store
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6a9991cd4a0a3fce630b1c7be64650c79daf74e6
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7706627"
---
# <a name="accessibility-in-the-store"></a>Accessibilité dans le Store  



Décrit la configuration requise pour la déclaration de votre applicationUWP (Universal Windows Platform ou plateforme Windows universelle) comme étant accessible dans le MicrosoftStore.

Lors de la soumission de votre application au MicrosoftStore à des fins de certification, vous pouvez la déclarer comme étant accessible. La déclaration de votre application comme accessible permet aux utilisateurs qui sont intéressés par les applications accessibles, par exemple les utilisateurs malvoyants, de l’identifier plus facilement. Les utilisateurs identifient les applications accessibles à l’aide du filtre **Accessible** lors d’une recherche dans le MicrosoftStore. La déclaration de votre application comme étant accessible ajoute également la balise **Accessible** à la description de votre application.

En déclarant votre application comme accessible, vous stipulez qu’elle dispose des [informations d’accessibilité de base](basic-accessibility-information.md) dont les utilisateurs ont besoin pour des scénarios essentiels utilisant un ou plusieurs des éléments suivants :

* le clavier ;
* un thème à contraste élevé;
* un paramètre de points par pouce(ppp) variable;
* une technologie d’assistance commune telle que les fonctionnalités d’accessibilité Windows, notamment le Narrateur, la Loupe et le Clavier visuel.

Vous devez déclarer votre application comme accessible si vous l’avez créée et testée pour l’accessibilité. Cela signifie que vous avez effectué les opérations suivantes:

* défini toutes les informations d’accessibilité pertinentes pour les éléments d’interface utilisateur, notamment le nom, le rôle, la valeur, etc.;
* implémenté l’accessibilité totale du clavier, ce qui permet à l’utilisateur:
    * d’accomplir les scénarios d’application principaux en utilisant uniquement le clavier;
    * navigué à l’aide de la touche Tab entre les éléments de l’interface utilisateur dans un ordre logique;
    * navigué entre les éléments de l’interface utilisateur dans un contrôle, à l’aide des touches de direction;
    * d’utiliser des raccourcis clavier pour atteindre les fonctionnalités essentielles de l’application.
    * d’utiliser les mouvements tactiles du Narrateur comme équivalents de la touche Tab et des touches de direction pour les appareils sans clavier.
* vérifié que l’interface utilisateur de votre application est visuellement accessible, qu’elle a un coefficient de contraste de texte minimal de 4,5 pour 1, qu’elle n’utilise pas simplement la couleur pour transmettre des informations, etc. ;
* utilisé des outils de test de l’accessibilité tels qu’[**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) et [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986) afin de vérifier votre implémentation de l’accessibilité, et résolu toutes les erreurs de priorité 1 signalées par ces outils ;
* vérifié les scénarios principaux de votre application de bout en bout à l’aide du Narrateur, de la Loupe, du Clavier visuel, d’un thème à contraste élevé et de paramètres haute résolution ajustés.

Pour obtenir un examen de cette procédure et des liens vers des ressources qui vous aideront à les appliquer, voir [Liste de vérification de l’accessibilité](accessibility-checklist.md).

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes    
* [Accessibilité](accessibility.md) 
