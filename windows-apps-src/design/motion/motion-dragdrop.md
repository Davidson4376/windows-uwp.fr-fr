---
author: mijacobs
Description: Use drag-and-drop animations when users move objects, such as moving an item within a list, or dropping an item on top of another.
title: Animations de glissement dans les applications UWP
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d271d0b9c8e7ce73835457789aca3fa2cb5eda97
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5747206"
---
# <a name="drag-animations"></a>Animations de glissement




Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.

> **API importantes**: [**classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


**Animation de début du glissement**

-   Utilisez l’animation de début du glissement quand l’utilisateur commence à déplacer un objet.
-   Insérez les objets affectés dans l’animation seulement si d’autres objets peuvent être affectés par l’opération de glisser-déplacer.
-   Utilisez l’animation de fin du glissement pour terminer toute séquence d’animation qui a commencé par l’animation de début du glissement. Le changement de taille provoqué par l’animation de début du glissement dans l’objet déplacé est alors inversé.

**Animation de fin du glissement**

-   Utilisez l’animation de fin du glissement quand l’utilisateur dépose un élément déplacé.
-   Utilisez l’animation de fin du glissement en combinaison avec les animations d’ajout et de suppression pour les listes.
-   Incluez les objets affectés dans l’animation de fin du glissement si et seulement si vous avez inclus ces mêmes objets affectés dans l’animation de début du glissement.
-   N’utilisez pas l’animation de fin du glissement si vous n’avez pas utilisé l’animation de début du glissement. Vous devez utiliser les deux animations pour que tous les objets retrouvent leur taille d’origine une fois la séquence de glissement terminée.

**Animation de glissement entre les objets**

-   Utilisez l’animation de glissement entre les objets quand l’utilisateur fait glisser la source du glissement dans une zone de dépôt où elle peut être placée entre deux objets.
-   Choisissez un emplacement où il est possible de déposer l’objet. Cet emplacement ne doit pas être trop petit, sinon l’utilisateur rencontrera des difficultés pour déposer la source du glissement.
-   La direction recommandée pour déplacer les objets affectés afin d’afficher la zone de dépôt est l’étirement direct les uns des autres. Qu’il s’agisse d’un mouvement vertical ou horizontal dépend de l’orientation des objets affectés les uns par rapport aux autres.
-   N’utilisez pas l’animation de glissement entre les objets, s’il n’est pas possible de déposer la source du glissement dans une zone. L’animation de glissement entre les objets indique à l’utilisateur que la source du glissement peut être déposée entre les objets affectés.

**Animation de glissement entre les objets par éloignement**

-   Utilisez l’animation de glissement entre les objets par éloignement quand l’utilisateur éloigne un objet d’une zone où il aurait pu être placé entre deux autres objets.
-   N’utilisez pas l’animation de glissement entre les objets par éloignement si vous n’avez pas d’abord utilisé l’animation de glissement entre les objets.


## <a name="related-articles"></a>Articles connexes

**Pour les développeurs**
* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de séquences de glisser-déplacer](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [Démarrage rapide: Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**Classe DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**Classe DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




