---
author: mijacobs
Description: Use pop-up animations to show and hide pop-up UI for flyouts or custom pop-up UI elements. Pop-up elements are containers that appear over the app's content and are dismissed if the user taps or clicks outside of the pop-up element.
title: Animations contextuelles d’interface utilisateur dans les applications UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcec5b62f400f2eb1410a61615d550f26520357f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1651938"
---
# <a name="pop-up-ui-animations"></a>Animations d’interface utilisateur contextuelle



Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.

> **API importantes**: [**classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383), [**classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations contextuelles pour afficher ou masquer des éléments d’interface utilisateur contextuels personnalisés qui ne font pas partie de la page de l’application proprement dite. Les contrôles courants fournis par Windows intègrent déjà ces animations.
-   N’utilisez pas d’animation contextuelle pour les info-bulles ou les boîtes de dialogue.
-   Ne recourez pas non plus aux animations contextuelles pour afficher ou masquer un élément d’interface utilisateur au sein du contenu principal de votre application; n’utilisez ces animations que pour afficher ou masquer un conteneur contextuel apparaissant au-dessus du contenu principal de l’application.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de l’interface utilisateur contextuelle](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Démarrage rapide: Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**Classe PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




