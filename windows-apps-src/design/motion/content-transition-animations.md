---
author: mijacobs
Description: Content transition animations let you change the content of an area of the screen while keeping the container or background constant. New content fades in. If there is existing content to be replaced, that content fades out.
title: Recommandations en matière d’animations de transition de contenu
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 14d5120632833e91e82ed7dd717ba04a9abb0efb
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5684440"
---
# <a name="content-transition-animations"></a>Animations de transition de contenu



Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît.

> **API importantes**: [**Classe ContentThemeTransition (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez une animation d’ouverture lorsque vous devez insérer un ensemble de nouveaux éléments dans un conteneur vide. Par exemple, après le chargement initial d’une application, une partie de son contenu est susceptible de ne pas pouvoir s’afficher immédiatement. Une fois le contenu prêt à l’affichage, utilisez une animation de transition de contenu pour activer son affichage.
-   Utilisez les transitions de contenu pour remplacer un ensemble de contenu par un autre résidant déjà dans le même conteneur sur la zone d’affichage.
-   Lorsque vous intégrez un nouveau contenu, faites glisser ce dernier de bas en haut pour l’insérer dans l’affichage dans le sens contraire du flux de pages général ou du sens de lecture.
-   Introduisez le nouveau contenu de manière logique, par exemple le contenu le plus important en dernier.
-   Si vous avez plusieurs conteneurs dont le contenu doit être actualisé, déclenchez l’intégralité des animations de transition simultanément sans aucune étape ou délai.
-   N’utilisez pas d’animations de transition de contenu quand la page entière est en cours de modification. Dans ce cas, utilisez plutôt les animations de transition entre les pages.
-   N’utilisez pas d’animation de transition de contenu si le contenu est uniquement en cours d’actualisation. Les animations de transition de contenu sont destinées à représenter du mouvement. Pour les actualisations, utilisez les animations en fondu.



## <a name="related-articles"></a>Articles connexes

**Pour les développeurs (XAML)**
* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de transitions de contenu](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Démarrage rapide: animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 




