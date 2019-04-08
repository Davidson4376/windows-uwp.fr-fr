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
ms.openlocfilehash: c7ca332b73aba067c2ae003d458e8d0d97c7a7e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642834"
---
# <a name="add-and-delete-animations"></a>Ajout et suppression d’animations



Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche.

> **API importantes** : [**Classe de AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations de liste pour ajouter un nouvel élément unique à un ensemble d’éléments existants, par exemple, quand un nouveau courrier électronique arrive ou quand une nouvelle photo est importée dans un ensemble existant.
-   Utilisez les animations de liste pour ajouter plusieurs éléments à la fois à un ensemble, par exemple, si vous importez un nouvel ensemble de photos dans une collection existante. L’ajout ou la suppression de plusieurs éléments doit s’effectuer en une seule opération, sans délai entre chaque objet.
-   Utilisez les animations d’ajout et de suppression dans la liste en tant que paire. Chaque fois que vous faites appel à l’une de ces animations, utilisez l’autre pour effectuer l’action opposée correspondante.
-   Utilisez les animations de liste avec une liste d’éléments à laquelle vous ajoutez ou supprimez un élément ou un groupe d’éléments en même temps.
-   N’utilisez pas d’animation de liste pour afficher ou supprimer un conteneur. Ces animations sont conçues pour être employées sur les membres d’une collection ou d’un ensemble déjà affiché à l’écran. Utilisez les animations de menu contextuel pour afficher ou masquer un conteneur temporaire au premier plan de la surface de l’application. Utilisez les animations de transition de contenu pour afficher ou remplacer un conteneur faisant partie de la surface de l’application.
-   N’utilisez pas d’animation de liste sur un ensemble entier d’éléments. Utilisez les animations de transition de contenu pour ajouter ou supprimer une collection entière dans votre conteneur.



## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animer la liste des ajouts et suppressions](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [Démarrage rapide : Animation de votre interface utilisateur à l’aide de la bibliothèque d’animations](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe de AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 




