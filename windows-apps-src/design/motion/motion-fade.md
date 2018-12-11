---
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: Animations en fondu dans les applications UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d3fee78f3608466f588a79d2811f1464e27a0ab
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872350"
---
# <a name="fade-animations"></a>Animations en fondu



Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.

> **API importantes**: [**classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298), [**classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Lorsque votre application effectue la transition entre des éléments non liés ou riches en texte, utilisez une disparition en fondu suivie d’une apparition en fondu. L’objet sortant disparaît ainsi entièrement avant que l’objet entrant ne soit visible.
-   Faites apparaître en fondu le ou les éléments entrants au-dessus des éléments sortants si leur taille reste constante et si vous voulez que l’utilisateur ait l’impression de voir le même élément. Une fois l’apparition en fondu terminée, vous pouvez simplement supprimer l’élément sortant. Cette option n’est viable que lorsque l’élément sortant est entièrement recouvert par l’élément entrant.
-   N’utilisez pas d’animations en fondu pour ajouter ou supprimer des éléments dans une liste. À la place, utilisez les animations de liste créées à cet effet.
-   Évitez d’utiliser des animations en fondu pour modifier l’intégralité du contenu d’une page. Utilisez plutôt les animations de transition entre les pages créées à cet effet.
-   Une disparition en fondu vous permet de supprimer un élément de façon subtile.
## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de fondus](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Démarrage rapide: Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




