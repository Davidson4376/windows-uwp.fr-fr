---
author: QuinnRadich
title: Quelles sont les nouveautés dans la documentation Windows en août 2018 - développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos, des exemples et des conseils aux développeurs ont été ajoutées à la documentation du développeur Windows 10 août 2018.
keywords: Nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, août
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5522144"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Quelles sont les nouveautés dans la documentation du développeur Windows en août 2018

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations de fonctionnalités, conseils aux développeurs et des vidéos suivantes ont été mis à disposition au cours du mois d’août.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="design"></a>Conception

Les fonctionnalités suivantes ont été ajoutées à Windows les builds Insider Preview, disponibles via le programme [Windows Insider](https://insider.windows.com/) .

* La [Bibliothèque de l’interface utilisateur de Windows](https://aka.ms/winui-docs) est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments interfact d’utilisateur pour les applications UWP. Ces packages sont également compatible avec les versions antérieures de Windows 10, afin que votre application fonctionne même si vos utilisateurs n’ont pas la dernière version du système d’exploitation.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [bouton partagé](../design/controls-and-patterns/buttons.md#create-a-split-button)et [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fournissent des contrôles de bouton avec des fonctionnalités spécialisées pour améliorer l’interface utilisateur de votre application.

![Un bouton partagé pour la sélection de couleur de premier plan](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView prend désormais en charge la [navigation en haut](../design/controls-and-patterns/navigationview.md), pour les cas dans lesquels votre application dispose d’un petit nombre d’options de navigation et requiert davantage d’espace pour le contenu de votre application.

* TreeView a été amélioré pour prendre en charge [liaison de données, les modèles, d’élément et de glisser -déplacer.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Infrastructure de prise en charge de package

L’infrastructure de prise en charge de package est un kit open source qui vous permet d’appliquer des correctifs à votre application win32 lorsque vous n’avez pas accès au code source, afin qu’il puisse exécuter dans un conteneur MSIX.

Pour en savoir plus, voir [runtime d’appliquer des correctifs à un package MSIX à l’aide de l’infrastructure de prise en charge de Package](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="web-api-extensions"></a>Extensions de l’API Web

Liste des [extensions d’API Microsoft héritées](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a été ajoutée à la documentation Mozilla Developer Network pour le développement entre les navigateurs web. Ces extensions d’API sont propres à Internet Explorer ou Microsoft Edge et complément les informations existantes sur le support de navigateur et de compatibilité dans la documentation du web MDN. Microsoft hérités [extensions CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) et [JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sont également disponibles, et vous pouvez trouver web riches informations sur les API à partir de MDN exposées directement en [Code Visual Studio.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ / exemples de Code C++

Nous avons ajouté 250 [C++ / WinRT](../cpp-and-winrt-apis/index.md) code descriptions vers des rubriques dans notre documentation, qui l’accompagne existant C + / exemples de code CX.

### <a name="project-rome"></a>Projet «Rome»

Le site de [documentation du projet «Rome»](https://docs.microsoft.com/windows/project-rome/) a été réorganisé dans une approche de la fonctionnalité de priorité. Cela doit facilitent pour les développeurs de rechercher qu’ils recherchent et pour implémenter des fonctionnalités de leur choix sur plusieurs plateformes.

## <a name="videos"></a>Vidéos

### <a name="xbox-live-unity-plugin"></a>Plug-in Xbox Live Unity

Le plug-in Xbox Live pour Unity prend en charge pour l’ajout de signature de Xbox Live, statistiques, listes d’amis, stockage cloud et les classements à votre titre. [Regardez la vidéo](https://youtu.be/fVQZ-YgwNpY) pour en savoir plus, puis [Télécharger le package GitHub](https://aka.ms/UnityPlugin) pour commencer.

### <a name="one-dev-question"></a>Question sur le développement

Dans la série de vidéos Question sur le développement, les développeurs Microsoft longtime couvrent une série de questions sur le développement Windows, la culture de l’équipe et historique. Voici les questions les plus récentes que nous avons répondu à!

Raymond Chen:

* [Comment le noyau sait-il quand redémarrer un pilote vidéo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Quel est l’histoire derrière l’objet Burgermaster dans Windows?](https://youtu.be/0TDSbyAIvX0)