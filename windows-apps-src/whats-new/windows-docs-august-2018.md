---
author: QuinnRadich
title: Nouveautés dans les documents Windows 2018 août - pour développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos, des exemples et des conseils pour les développeurs ont été ajoutés à la documentation pour les développeurs Windows 10 août 2018.
keywords: Quelles sont les nouveautés, mise à jour, fonctionnalités, des conseils pour les développeurs, Windows 10 août
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815968"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Nouveautés dans la documentation de développeur Windows 2018 août

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les vues d’ensemble de la fonctionnalité, conseils pour les développeurs et des vidéos suivantes ont été apportées disponibles dans le mois d’août.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="design"></a>Conception

Les fonctionnalités suivantes ont été ajoutées à Windows versions Preview initiés, disponibles via le programme [Windows internes](https://insider.windows.com/) .

* La [Bibliothèque de l’interface utilisateur Windows](https://aka.ms/winui-docs) est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments interfact utilisateur pour les applications UWP. Ces packages sont également compatible avec les versions antérieures de Windows 10, afin que votre application fonctionne même si vos utilisateurs n’ont pas la dernière version du système d’exploitation.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [bouton partagé](../design/controls-and-patterns/buttons.md#create-a-split-button)et [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) fournissent des contrôles de bouton avec des fonctionnalités spécialisées pour améliorer l’interface utilisateur de votre application.

![Un bouton partagé pour sélectionner la couleur de premier plan](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView prend désormais en charge la [navigation supérieure](../design/controls-and-patterns/navigationview.md), pour les cas où votre application a un plus petit nombre d’options de navigation et requièrent davantage d’espace pour le contenu de votre application.

* TreeView a été amélioré pour prendre en charge [liaison de données, les modèles, d’élément et glisser -déplacer.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Infrastructure de prise en charge de package

L’infrastructure de prise en charge de package est un kit open source qui vous permet d’appliquer des correctifs pour votre application win32 lorsque vous n’avez pas accès au code source, afin qu’il peut s’exécuter dans un conteneur MSIX.

Pour plus d’informations, voir [Appliquer runtime résout un package MSIX à l’aide de l’infrastructure de prise en charge de Package](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="web-api-extensions"></a>Extensions Web API

Liste des [extensions d’API Microsoft héritées](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a été ajoutée à la documentation MSDN Mozilla pour le développement élargie des navigateurs web. Ces extensions d’API sont uniques pour Internet Explorer ou Microsoft Edge et viennent s’ajouter aux informations existantes sur la compatibilité et navigateur prise en charge dans les documents web MDN. Hérité Microsoft [extensions CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) et [JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sont également disponibles, et vous pouvez trouver web riche informations de l’API de MDN exposées directement dans [Code Visual Studio.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + / exemples de WinRT Code

Nous avons ajouté 250 [C + / WinRT](../cpp-and-winrt-apis/index.md) code annonces à des rubriques de notre documentation accompagnant existant C + / exemples de code CX.

### <a name="project-rome"></a>Projet «Rome»

Le site de [projet Rome documents](https://docs.microsoft.com/windows/project-rome/) a été réorganisé dans une approche préalable à la fonctionnalité. Cela doit faciliter pour les développeurs pour trouver ce qu’ils cherchent et pour implémenter les fonctionnalités de leur choix entre plusieurs plates-formes.

## <a name="videos"></a>Vidéos

### <a name="xbox-live-unity-plugin"></a>Plug-in de l’unité Xbox Live

Le plug-in Xbox Live unité contient la prise en charge pour l’ajout de signature Xbox Live, stats, listes d’amis, stockage en nuage et ajouterons au titre. [Regarder la vidéo](https://youtu.be/fVQZ-YgwNpY) pour en savoir plus, puis [Téléchargez le package de référentiels](https://aka.ms/UnityPlugin) pour commencer.

### <a name="one-dev-question"></a>Une Question pour les développeurs

Dans la série de vidéos une Question pour les développeurs, les développeurs Microsoft longtime couvrent une série de questions sur le développement Windows, la culture de l’équipe et l’historique. Voici les questions le plus récent que nous avons répondu!

Raymond Chen:

* [Comment le noyau que quand redémarrer un pilote vidéo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Quel est l’article derrière l’objet Burgermaster dans Windows?](https://youtu.be/0TDSbyAIvX0)