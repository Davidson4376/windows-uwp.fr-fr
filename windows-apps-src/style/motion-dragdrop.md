---
author: mijacobs
Description: "Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre."
title: Animations de glissement dans les applications UWP
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: c975ba3d03c06009710f4f57edf8a4cc517ad90c

---

# Animations de glissement




Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.

**API importantes**

-   [**Classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)


## Pratiques conseillées et déconseillées


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


## Articles connexes

**Pour les développeurs (XAML)**
* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de séquences de glisser-déplacer](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [Démarrage rapide : animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**Classe DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**Classe DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243180)


 







<!--HONumber=Jun16_HO4-->


