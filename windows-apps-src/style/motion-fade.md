---
Description: Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.
title: Animations en fondu dans les applications UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
---

# Afficher et masquer les animations en fondu


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.

**API importantes**

-   [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
-   [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## Pratiques conseillées et déconseillées


-   Lorsque votre application effectue la transition entre des éléments non liés ou riches en texte, utilisez une disparition en fondu suivie d’une apparition en fondu. L’objet sortant disparaît ainsi entièrement avant que l’objet entrant ne soit visible.
-   Faites apparaître en fondu le ou les éléments entrants au-dessus des éléments sortants si leur taille reste constante et si vous voulez que l’utilisateur ait l’impression de voir le même élément. Une fois l’apparition en fondu terminée, vous pouvez simplement supprimer l’élément sortant. Cette option n’est viable que lorsque l’élément sortant est entièrement recouvert par l’élément entrant.
-   N’utilisez pas d’animations en fondu pour ajouter ou supprimer des éléments dans une liste. À la place, utilisez les animations de liste créées à cet effet.
-   Évitez d’utiliser des animations en fondu pour modifier l’intégralité du contenu d’une page. Utilisez plutôt les animations de transition entre les pages créées à cet effet.
-   Une disparition en fondu vous permet de supprimer un élément de façon subtile.
## Articles connexes

**Pour les développeurs (XAML)**
* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de fondus](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Démarrage rapide : animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 






<!--HONumber=Mar16_HO3-->


