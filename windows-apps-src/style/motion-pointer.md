---
author: mijacobs
Description: "Utilisez les animations de pointeur pour fournir un retour visuel lorsqu’un utilisateur appuie sur un élément."
title: Animations de clic avec le pointeur dans les applications UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
ms.openlocfilehash: c208b67829e2053302ec0cc7c6da013b89cee8b3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pointer-click-animations"></a>Animations de clic avec le pointeur

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Utilisez les animations de pointeur pour fournir un retour visuel lorsqu’un utilisateur appuie sur un élément. L’animation de pointeur vers le bas réduit et incline légèrement l’élément sélectionné. Elle est lue lorsque l’utilisateur appuie pour la première fois sur un élément. L’animation de pointeur vers le haut, qui restaure l’élément à sa position d’origine, est lue quand l’utilisateur relâche le pointeur.


<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)</li>
<li>[**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Lorsque vous utilisez une animation de pointeur vers le haut, déclenchez immédiatement l’animation dès que l’utilisateur relâche le pointeur. Ceci permet de fournir un retour instantané à l’utilisateur, qui sait ainsi que son action a été reconnue, même si l’action déclenchée par l’appui (telle que la navigation vers une nouvelle page) est plus lente à se produire.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de clics de pointeur](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Démarrage rapide: Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




