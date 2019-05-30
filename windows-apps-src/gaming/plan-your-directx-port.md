---
title: Planifier votre portage DirectX
description: 'Planifiez le portage de votre jeu DirectX 9 sur DirectX 11 et la plateforme Windows universelle (UWP) : mettez à niveau votre code graphique et préparez votre jeu pour l’environnement Windows Runtime.'
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx , portage
ms.localizationpriority: medium
ms.openlocfilehash: 247c7cb05027520cb7a39e04ff65579297b66dc9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368301"
---
# <a name="plan-your-directx-port"></a>Planifier votre portage DirectX



**Résumé**

-   Planifier votre portage DirectX
-   [Modifications importantes par rapport à partir de Direct3D 9 à Direct3D 11](understand-direct3d-11-1-concepts.md)
-   [Mappage de fonction](feature-mapping.md)


Planifiez le portage de votre jeu DirectX 9 sur DirectX 11 et la plateforme Windows universelle (UWP) : mettez à niveau votre code graphique et préparez votre jeu pour l’environnement Windows Runtime.

## <a name="plan-to-port-graphics-code"></a>Planifier le portage du code graphique


Avant de commencer le portage de votre jeu sur WPU, il est essentiel de vous assurer que ce jeu ne comporte plus d’éléments issus de Direct3D 8. Vérifiez notamment que votre jeu ne contient plus de références au pipeline de fonctions fixes. Pour obtenir la liste complète des fonctionnalités déconseillées, y compris le pipeline de fonctions fixes, voir [Fonctionnalités déconseillées](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

La mise à niveau de Direct3D 9 vers Direct3D 11 ne se limite pas à une simple opération de recherche et remplacement. Vous devez savoir en quoi diffèrent ces versions sur le plan du périphérique Direct3D, du contexte de périphérique et de l’infrastructure graphique, mais aussi vous documenter sur les autres modifications majeures qui ont été apportées depuis la version Direct3D 9. Dans cette optique, nous vous recommandons de commencer par lire les autres rubriques de cette section.

Vous devez remplacer les bibliothèques d’applications auxiliaires D3DX et DXUT par vos propres bibliothèques d’applications auxiliaires ou par des outils de la communauté. Pour plus d’informations, voir la section [Mappage des fonctionnalités](feature-mapping.md).

> **Remarque**    vous pouvez utiliser la [DirectX Tool Kit](https://go.microsoft.com/fwlink/p/?LinkID=248929) ou [DirectXTex](https://go.microsoft.com/fwlink/p/?LinkID=248926) pour remplacer certaines fonctionnalités qui ont été précédemment fournies par D3DX et DXUT.

 

Nuanceurs écrites en langage assembleur doivent être mis à niveau au langage HLSL à l’aide du niveau de modèle 4 nuanceur 9\_1 ou 9\_3 fonctionnalités et des nuanceurs écrites pour la bibliothèque d’effets doivent être mis à jour vers une version plus récente de la syntaxe du langage HLSL. Pour plus d’informations, voir la section [Mappage des fonctionnalités](feature-mapping.md).

Familiarisez-vous avec les différents [niveaux de fonctionnalité Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro). Ces niveaux de fonctionnalité permettent de classer les divers matériels vidéo selon des ensembles définis de fonctionnalités prises en charge. Chaque ensemble correspond globalement à une version de Direct3D (versions 9.1 à 11.2). Tous les niveaux de fonctionnalité s’appliquent à l’API DirectX 11.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planifier le portage du code d’interface utilisateur Win32 sur CoreWindow


Les applications UWP s’exécutent dans une fenêtre créée pour un conteneur d’application, appelée [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). Votre jeu contrôle cette fenêtre en héritant d’[**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView), qui nécessite moins d’informations d’implémentation détaillées qu’une fenêtre de bureau. La boucle principale de votre jeu sera définie dans la méthode [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run).

Le cycle de vie d’une application UWP diffère de celui d’une application de bureau. Vous devez prévoir des enregistrements fréquents du jeu. En effet, durant le déclenchement d’un événement de suspension, votre application ne dispose que d’un court délai pour arrêter l’exécution du code, et vous voulez vous assurer que le joueur pourra revenir à l’endroit du jeu où il était initialement aussitôt après la reprise de l’application. Vous devez enregistrer votre jeu aussi souvent que nécessaire pour permettre au joueur de continuer sa partie normalement après la reprise, tout en limitant le nombre d’enregistrements afin de ne pas dégrader la fréquence d’images ni provoquer d’interruption de son dans le jeu. Votre jeu aura peut-être besoin de charger son état lorsqu’il quittera l’état « arrêté ».

[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-progguide) peut avantageusement remplacer D3DXMath et XNAMath, en particulier si vous avez besoin d’une bibliothèque de fonctions mathématiques. DirectXMath fournit des types de données rapides et portables, ainsi que des types adaptés et groupés pour être utilisés avec des nuanceurs.

Les bibliothèques natives, telles que l’[API Interlocked](https://docs.microsoft.com/windows/desktop/Sync/what-s-new-in-synchronization), ont été étendues afin de prendre en charge les intrinsèques ARM. Si votre jeu fait appel à des API à blocage, vous pouvez continuer à les utiliser dans DirectX 11 et UWP.

Nos modèles et exemples de code utilisent de nouvelles fonctionnalités C++ que vous ne connaissez peut-être pas encore. Citons, par exemple, les méthodes asynchrones que vous pouvez utiliser avec des [**lambda expressions**](https://docs.microsoft.com/cpp/cpp/lambda-expressions-in-cpp) pour charger les ressources Direct3D sans bloquer le thread d’interface utilisateur.

Les deux concepts suivants vous seront souvent utiles :

-   Les références managées ([**l’opérateur ^** ](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)) et les [**classes managées**](https://docs.microsoft.com/cpp/windows/classes-and-structs-cpp-component-extensions) (classes de référence) sont des éléments fondamentaux de Windows Runtime. Vous aurez besoin d’utiliser des classes de référence managées pour assurer la communication avec les composants Windows Runtime, tels qu’[**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) (pour plus d’informations, voir la procédure pas à pas).
-   Si vous travaillez avec des interfaces COM Direct3D 11, optez pour le type de modèle [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) afin de faciliter l’utilisation des pointeurs COM.

 

 




