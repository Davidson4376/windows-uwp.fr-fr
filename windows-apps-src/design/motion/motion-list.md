---
Description: Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche.
title: Ajout et suppression d’animations dans les applications UWP
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 228fec1571ab3f9c01b4a8dd4084e19bc28dbfe7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366743"
---
# <a name="add-and-delete-animations"></a>Ajout et suppression d’animations



Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche.

> **API importantes** : [**Classe de AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition.)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations de liste pour ajouter un nouvel élément unique à un ensemble d’éléments existants, par exemple, quand un nouveau courrier électronique arrive ou quand une nouvelle photo est importée dans un ensemble existant.
-   Utilisez les animations de liste pour ajouter plusieurs éléments à la fois à un ensemble, par exemple, si vous importez un nouvel ensemble de photos dans une collection existante. L’ajout ou la suppression de plusieurs éléments doit s’effectuer en une seule opération, sans délai entre chaque objet.
-   Utilisez les animations d’ajout et de suppression dans la liste en tant que paire. Chaque fois que vous faites appel à l’une de ces animations, utilisez l’autre pour effectuer l’action opposée correspondante.
-   Utilisez les animations de liste avec une liste d’éléments à laquelle vous ajoutez ou supprimez un élément ou un groupe d’éléments en même temps.
-   N’utilisez pas d’animation de liste pour afficher ou supprimer un conteneur. Ces animations sont conçues pour être employées sur les membres d’une collection ou d’un ensemble déjà affiché à l’écran. Utilisez les animations de menu contextuel pour afficher ou masquer un conteneur temporaire au premier plan de la surface de l’application. Utilisez les animations de transition de contenu pour afficher ou remplacer un conteneur faisant partie de la surface de l’application.
-   N’utilisez pas d’animation de liste sur un ensemble entier d’éléments. Utilisez les animations de transition de contenu pour ajouter ou supprimer une collection entière dans votre conteneur.



## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animer la liste des ajouts et suppressions](https://docs.microsoft.com/previous-versions/windows/apps/jj649430(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur à l’aide de la bibliothèque d’animations](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition.)

 

 




