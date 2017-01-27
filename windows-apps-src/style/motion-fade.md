---
author: mijacobs
Description: "Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu."
title: Animations en fondu dans les applications UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: cbe1a4ca77f0d6e15b3d12b2b3a0f2f8363530ae

---

# <a name="fade-animations"></a>Afficher et masquer les animations en fondu

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)</li>
<li>[**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Lorsque votre application effectue la transition entre des éléments non liés ou riches en texte, utilisez une disparition en fondu suivie d’une apparition en fondu. L’objet sortant disparaît ainsi entièrement avant que l’objet entrant ne soit visible.
-   Faites apparaître en fondu le ou les éléments entrants au-dessus des éléments sortants si leur taille reste constante et si vous voulez que l’utilisateur ait l’impression de voir le même élément. Une fois l’apparition en fondu terminée, vous pouvez simplement supprimer l’élément sortant. Cette option n’est viable que lorsque l’élément sortant est entièrement recouvert par l’élément entrant.
-   N’utilisez pas d’animations en fondu pour ajouter ou supprimer des éléments dans une liste. À la place, utilisez les animations de liste créées à cet effet.
-   Évitez d’utiliser des animations en fondu pour modifier l’intégralité du contenu d’une page. Utilisez plutôt les animations de transition entre les pages créées à cet effet.
-   Une disparition en fondu vous permet de supprimer un élément de façon subtile.
## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de fondus](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 







<!--HONumber=Dec16_HO2-->


