---
Description: Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.
title: Animations de glissement dans les applications UWP
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07707160846f3d63c7d0c097fb7b84def08be9e7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366694"
---
# <a name="drag-animations"></a>Animations de glissement




Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.

> **API importantes** : [**Classe de DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation.)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


**Animation de début de glissement**

-   Utilisez l’animation de début du glissement quand l’utilisateur commence à déplacer un objet.
-   Insérez les objets affectés dans l’animation seulement si d’autres objets peuvent être affectés par l’opération de glisser-déplacer.
-   Utilisez l’animation de fin du glissement pour terminer toute séquence d’animation qui a commencé par l’animation de début du glissement. Le changement de taille provoqué par l’animation de début du glissement dans l’objet déplacé est alors inversé.

**Animation de fin de glissement**

-   Utilisez l’animation de fin du glissement quand l’utilisateur dépose un élément déplacé.
-   Utilisez l’animation de fin du glissement en combinaison avec les animations d’ajout et de suppression pour les listes.
-   Incluez les objets affectés dans l’animation de fin du glissement si et seulement si vous avez inclus ces mêmes objets affectés dans l’animation de début du glissement.
-   N’utilisez pas l’animation de fin du glissement si vous n’avez pas utilisé l’animation de début du glissement. Vous devez utiliser les deux animations pour que tous les objets retrouvent leur taille d’origine une fois la séquence de glissement terminée.

**Faites glisser entrez entre animations**

-   Utilisez l’animation de glissement entre les objets quand l’utilisateur fait glisser la source du glissement dans une zone de dépôt où elle peut être placée entre deux objets.
-   Choisissez un emplacement où il est possible de déposer l’objet. Cet emplacement ne doit pas être trop petit, sinon l’utilisateur rencontrera des difficultés pour déposer la source du glissement.
-   La direction recommandée pour déplacer les objets affectés afin d’afficher la zone de dépôt est l’étirement direct les uns des autres. Qu’il s’agisse d’un mouvement vertical ou horizontal dépend de l’orientation des objets affectés les uns par rapport aux autres.
-   N’utilisez pas l’animation de glissement entre les objets, s’il n’est pas possible de déposer la source du glissement dans une zone. L’animation de glissement entre les objets indique à l’utilisateur que la source du glissement peut être déposée entre les objets affectés.

**Faites glisser entre l’animation de congé**

-   Utilisez l’animation de glissement entre les objets par éloignement quand l’utilisateur éloigne un objet d’une zone où il aurait pu être placé entre deux autres objets.
-   N’utilisez pas l’animation de glissement entre les objets par éloignement si vous n’avez pas d’abord utilisé l’animation de glissement entre les objets.


## <a name="related-articles"></a>Articles connexes

**pour les développeurs**
* [Vue d’ensemble des animations](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animer des séquences de glisser-déplacer](https://docs.microsoft.com/previous-versions/windows/apps/jj649427(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur à l’aide de la bibliothèque d’animations](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**DragItemThemeAnimation class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation.)
* [**DropTargetItemThemeAnimation class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.droptargetitemthemeanimation.)
* [**DragOverThemeAnimation class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragoverthemeanimation.)


 




