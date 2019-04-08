---
title: Nouveautés dans Windows Docs août 2018 - développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos, des exemples et des instructions destinées aux développeurs ont été ajoutés à la documentation pour développeurs Windows 10 pour août 2018.
keywords: Quelles sont les nouveautés, mise à jour, des fonctionnalités, des instructions destinées aux développeurs, Windows 10, août
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616484"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Quelles sont les nouveautés dans la documentation du développeur Windows en août 2018

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations des fonctionnalités, Guide du développeur et vidéos suivants ont été apportées disponibles dans le mois d’août.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="design"></a>Concevoir

Les fonctionnalités suivantes ont été ajoutées à la Windows builds Insider Preview, disponibles via le [Windows Insider](https://insider.windows.com/) programme.

* Le [bibliothèque d’interface utilisateur Windows](https://aka.ms/winui-docs) est un ensemble de packages NuGet qui fournissent des contrôles et autres utilisateur éléments interfact, pour les applications UWP. Ces packages sont également compatible avec les versions antérieures de Windows 10, afin que votre application fonctionne même si vos utilisateurs n’ont pas la dernière version du système d’exploitation.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), et [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fournissent des contrôles de bouton avec des fonctionnalités spécialisées pour améliorer l’interface utilisateur de votre application.

![Un bouton partagé pour la sélection de couleur de premier plan](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView prend désormais en charge [Top navigation](../design/controls-and-patterns/navigationview.md), pour les cas dans lesquels votre application a un plus petit nombre d’options de navigation et nécessitent davantage d’espace pour le contenu de votre application.

* TreeView a été amélioré pour prendre en charge [liaison de données, modèles d’élément, puis faites-la glisser.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Framework de prise en charge de package

L’infrastructure de prise en charge de package est un kit open source qui vous permet d’appliquer des correctifs à votre application win32 lorsque vous n’avez pas accès au code source, afin qu’il peut s’exécuter dans un conteneur MSIX.

Pour plus d’informations, consultez [runtime d’appliquer des correctifs à un package MSIX à l’aide de l’infrastructure de prise en charge de Package](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="web-api-extensions"></a>Extensions de l’API Web

Une liste de [extensions héritées de l’API Microsoft](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a été ajouté à la documentation de Mozilla Developer Network pour le développement entre navigateurs web. Ces extensions d’API sont uniques à Internet Explorer ou Microsoft Edge et complément les informations existantes sur la prise en charge la compatibilité et le navigateur en mode furtif dans la documentation de web MDN. Microsoft hérité [extensions CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) et [extensions JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sont également disponibles et vous trouverez web riches informations sur l’API à partir de MDN exposés directement dans [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ / c++ / exemples de Code de WinRT

Nous avons ajouté des 250 [C++ / c++ / WinRT](../cpp-and-winrt-apis/index.md) annonces vers des rubriques dans notre documentation d’accompagnement existant C + de code c++ / exemples de code CX.

### <a name="project-rome"></a>Projet Rome

Le [docs de Project Rome](https://docs.microsoft.com/windows/project-rome/) site a été réorganisé dans une approche de la fonctionnalité en premier. Cela doit faciliter pour les développeurs à trouver ce qu’ils cherchent et pour implémenter des fonctionnalités de leur choix sur plusieurs plateformes.

## <a name="videos"></a>Vidéos

### <a name="xbox-live-unity-plugin"></a>Plug-in de Xbox Live Unity

Le plug-in Xbox Live pour Unity contient la prise en charge pour l’ajout de signature de Xbox Live, statistiques, listes d’amis, stockage cloud et leaderboards à votre titre. [Regardez la vidéo](https://youtu.be/fVQZ-YgwNpY) pour en savoir plus, puis [télécharger le package GitHub](https://aka.ms/UnityPlugin) pour commencer.

### <a name="one-dev-question"></a>Une Question de développement

Dans la série de vidéos d’une Question de développement, les développeurs Microsoft contribue depuis longtemps couvrent une série de questions sur le développement Windows, la culture de l’équipe et l’historique. Voici les dernières questions que nous avons une réponse !

Raymond Chen :

* [Comment le noyau sait quand redémarrer un pilote vidéo ?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman :

* [Qu’est l’histoire situé derrière l’objet Burgermaster dans Windows ?](https://youtu.be/0TDSbyAIvX0)