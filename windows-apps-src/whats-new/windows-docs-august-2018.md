---
title: Nouveautés apportées dans la documentation Windows en août 2018 - Développer des applications UWP
description: De nouvelles fonctionnalités, des vidéos, des exemples et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows 10 en août 2018.
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, août
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63780242"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Nouveautés apportées dans la documentation du développeur Windows en août 2018

La documentation du développeur Windows est constamment mise à jour avec des informations sur les nouvelles fonctionnalités mises à la disposition des développeurs sur la plateforme Windows. Les présentations de fonctionnalités, conseils aux développeurs et vidéos ci-dessous ont été mis à disposition au mois d'août.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="design"></a>Concevoir

Les fonctionnalités suivantes ont été ajoutées aux builds Windows Insider Preview, disponibles via le programme [Windows Insider](https://insider.windows.com/).

* La [bibliothèque de l'interface utilisateur de Windows](https://aka.ms/winui-docs) est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments d'interface utilisateur pour les applications UWP. Ces packages sont également compatibles avec les versions antérieures de Windows 10, pour que votre application fonctionne même si vos utilisateurs ne disposent pas de la dernière version du système d'exploitation.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) et [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fournissent des commandes de bouton avec des fonctionnalités spécialisées permettant d'améliorer l'interface utilisateur de votre application.

![Un bouton partagé pour sélectionner la couleur de premier plan](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView prend désormais en charge la [Navigation supérieure](../design/controls-and-patterns/navigationview.md), pour les cas où votre application dispose d'un nombre réduit d'options de navigation et requiert davantage d'espace pour son contenu.

* TreeView a été amélioré pour prendre en charge [la liaison de données, les modèles d'élément et le glisser-déplacer.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Framework de prise en charge de package

La Framework de prise en charge de package est un kit open source qui vous aide à appliquer des correctifs à votre application win32 lorsque vous n'avez pas accès au code source, afin qu'il puisse être exécuté dans un conteneur MSIX.

Pour en savoir plus, consultez [Appliquer des correctifs à l'exécution à un package MSIX à l'aide du Framework de prise en charge de package](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="web-api-extensions"></a>Extensions d'API web

Une liste d'[extensions d'API Microsoft héritées](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a été ajoutée à la documentation de Mozilla Developer Network pour le développement web entre navigateurs. Ces extensions d'API sont propres à Internet Explorer ou à Microsoft Edge et complètent les informations existantes sur la compatibilité et la prise en charge des navigateurs dans la documentation web de MDN. Des extensions [CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) et [JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) héritées de Microsoft sont également disponibles. Et vous trouverez des informations MDN détaillées sur les API web dans [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>Exemples de code C++/WinRT

Nous avons ajouté 250 listes de code [C++/WinRT](../cpp-and-winrt-apis/index.md) à certaines rubriques de notre documentation, pour accompagner les exemples de code C++/CX existants.

### <a name="project-rome"></a>Projet Rome

Le site [Documents du projet Rome](https://docs.microsoft.com/windows/project-rome/) a été réorganisé pour adopter une approche axée sur les fonctionnalités. Cela devrait permettre aux développeurs d'accéder plus facilement à ce qu'ils cherchent et d'implémenter les fonctionnalités de leur choix sur différentes plateformes.

## <a name="videos"></a>Vidéos

### <a name="xbox-live-unity-plugin"></a>Plug-in Xbox Live Unity

Le plug-in Xbox Live pour Unity prend en charge l'ajout de signatures, de statistiques, de listes d'amis, de stockage cloud et de classements Xbox Live à votre titre. [Regardez la vidéo](https://youtu.be/fVQZ-YgwNpY) pour en savoir plus, puis [téléchargez le package GitHub](https://aka.ms/UnityPlugin) pour démarrer.

### <a name="one-dev-question"></a>One Dev Question

Dans le cadre de la série de vidéos One Dev Question, des développeurs Microsoft expérimentés répondent à des questions sur le développement, la culture d'équipe et l'histoire de Windows. Voici les dernières questions auxquelles nous avons répondu !

Raymond Chen :

* [Comment le noyau sait-il quand redémarrer un pilote vidéo ?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman :

* [Quelle est l'histoire de l'objet Burgermaster dans Windows ?](https://youtu.be/0TDSbyAIvX0)