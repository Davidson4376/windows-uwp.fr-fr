---
author: mijacobs
Description: "Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît."
title: "Recommandations en matière d’animations de transition de contenu"
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: dc233ef821259701eb93c09d1bdefcfa3d9bb69f

---

# Animations de transition de contenu





**API importantes**

-   [**Classe ContentThemeTransition (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)
-   [**Fonction enterContent (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh701582)

Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Pratiques conseillées et déconseillées


-   Utilisez une animation d’ouverture lorsque vous devez insérer un ensemble de nouveaux éléments dans un conteneur vide. Par exemple, après le chargement initial d’une application, une partie de son contenu est susceptible de ne pas pouvoir s’afficher immédiatement. Une fois le contenu prêt à l’affichage, utilisez une animation de transition de contenu pour activer son affichage.
-   Utilisez les transitions de contenu pour remplacer un ensemble de contenu par un autre résidant déjà dans le même conteneur sur la zone d’affichage.
-   Lorsque vous intégrez un nouveau contenu, faites glisser ce dernier de bas en haut pour l’insérer dans l’affichage dans le sens contraire du flux de pages général ou du sens de lecture.
-   Introduisez le nouveau contenu de manière logique, par exemple le contenu le plus important en dernier.
-   Si vous avez plusieurs conteneurs dont le contenu doit être actualisé, déclenchez l’intégralité des animations de transition simultanément sans aucune étape ou délai.
-   N’utilisez pas d’animations de transition de contenu quand la page entière est en cours de modification. Dans ce cas, utilisez plutôt les animations de transition entre les pages.
-   N’utilisez pas d’animation de transition de contenu si le contenu est uniquement en cours d’actualisation. Les animations de transition de contenu sont destinées à représenter du mouvement. Pour les actualisations, utilisez les animations en fondu.



## <span id="related_topics"></span>Articles connexes

**Pour les développeurs (XAML)**
* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de transitions de contenu](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Démarrage rapide: animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 







<!--HONumber=Jun16_HO4-->


