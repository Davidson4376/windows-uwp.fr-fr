---
Description: Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.
title: Animations contextuelles d’interface utilisateur dans les applications UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d79c369e14236b827bdc18aba6c74349528728b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635174"
---
# <a name="pop-up-ui-animations"></a>Animations d’interface utilisateur contextuelle



Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.

> **API importantes**: [**PopInThemeAnimation class**](https://msdn.microsoft.com/library/windows/apps/br210383), [**PopupThemeTransition class**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations contextuelles pour afficher ou masquer des éléments d’interface utilisateur contextuels personnalisés qui ne font pas partie de la page de l’application proprement dite. Les contrôles courants fournis par Windows intègrent déjà ces animations.
-   N’utilisez pas d’animation contextuelle pour les info-bulles ou les boîtes de dialogue.
-   Ne recourez pas non plus aux animations contextuelles pour afficher ou masquer un élément d’interface utilisateur au sein du contenu principal de votre application ; n’utilisez des animations contextuelles que pour afficher ou masquer un conteneur contextuel apparaissant au-dessus du contenu principal de l’application.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de l’interface utilisateur contextuelle](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Démarrage rapide : Animation de votre interface utilisateur à l’aide de la bibliothèque d’animations](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe de PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**PopOutThemeAnimation class**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Classe de PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




