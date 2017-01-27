---
author: mijacobs
Description: " Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche."
title: "Ajout et suppression d’animations dans les applications UWP"
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: e1fc86e5110da8926194e20947552337216d4a72

---

# <a name="add-and-delete-animations"></a>Ajout et suppression d’animations

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations de liste pour ajouter un nouvel élément unique à un ensemble d’éléments existants, par exemple, quand un nouveau courrier électronique arrive ou quand une nouvelle photo est importée dans un ensemble existant.
-   Utilisez les animations de liste pour ajouter plusieurs éléments à la fois à un ensemble, par exemple, si vous importez un nouvel ensemble de photos dans une collection existante. L’ajout ou la suppression de plusieurs éléments doit s’effectuer en une seule opération, sans délai entre chaque objet.
-   Utilisez les animations d’ajout et de suppression dans la liste en tant que paire. Chaque fois que vous faites appel à l’une de ces animations, utilisez l’autre pour effectuer l’action opposée correspondante.
-   Utilisez les animations de liste avec une liste d’éléments à laquelle vous ajoutez ou supprimez un élément ou un groupe d’éléments en même temps.
-   N’utilisez pas d’animation de liste pour afficher ou supprimer un conteneur. Ces animations sont conçues pour être employées sur les membres d’une collection ou d’un ensemble déjà affiché à l’écran. Utilisez les animations de menu contextuel pour afficher ou masquer un conteneur temporaire au premier plan de la surface de l’application. Utilisez les animations de transition de contenu pour afficher ou remplacer un conteneur faisant partie de la surface de l’application.
-   N’utilisez pas d’animation de liste sur un ensemble entier d’éléments. Utilisez les animations de transition de contenu pour ajouter ou supprimer une collection entière dans votre conteneur.



## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation d’ajouts et de suppressions dans la liste](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [Démarrage rapide : animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 







<!--HONumber=Dec16_HO2-->


