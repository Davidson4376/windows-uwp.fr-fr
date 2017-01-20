---
author: mijacobs
Description: "Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels."
title: "Animations contextuelles d’interface utilisateur dans les applications UWP"
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: cb5d70784e758b4e18092b75df9e0d77243af7fc

---

# <a name="pop-up-ui-animations"></a>Animations d’interface utilisateur contextuelle

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**Classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations contextuelles pour afficher ou masquer des éléments d’interface utilisateur contextuels personnalisés qui ne font pas partie de la page de l’application proprement dite. Les contrôles courants fournis par Windows intègrent déjà ces animations.
-   N’utilisez pas d’animation contextuelle pour les info-bulles ou les boîtes de dialogue.
-   Ne recourez pas non plus aux animations contextuelles pour afficher ou masquer un élément d’interface utilisateur au sein du contenu principal de votre application ; n’utilisez ces animations que pour afficher ou masquer un conteneur contextuel apparaissant au-dessus du contenu principal de l’application.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de l’interface utilisateur contextuelle](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**Classe PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 







<!--HONumber=Dec16_HO2-->


