---
author: mtoepke
title: 'Portage DirectX 11 : FAQ'
description: Réponses aux questions fréquemment posées sur le portage de jeux vers la plateforme Windows universelle (UWP).
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx11
ms.localizationpriority: medium
ms.openlocfilehash: 06a4c9b434afedabc17a48e9929da8dc4460fe03
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7298628"
---
# <a name="directx-11-porting-faq"></a>Portage DirectX11: FAQ




Réponses aux questions fréquemment posées sur le portage de jeux vers la plateforme Windows universelle (UWP).

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>Le portage de mon jeu va-t-il se résumer à une série d’opérations de copier-coller sur les méthodes API ou dois-je planifier un processus de portage plus réfléchi ?


Direct3D 11 est une mise à jour importante de Direct3D 9. Elle engendre plusieurs changements de paradigmes, y compris des API distinctes pour la carte graphique virtualisée et son contexte, ainsi qu’une nouvelle couche de polymorphisme pour les ressources de périphériques. Votre jeu peut toujours utiliser du matériel vidéo plus ou moins de la même façon, mais vous devrez découvrir la nouvelle architecture des API Direct3D 11 et mettre à jour chaque partie de votre code graphique pour utiliser les composants API appropriés. Voir [Concepts et considérations sur le portage](porting-considerations.md).

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>Quel est le rôle du nouveau contexte de périphérique ? Dois-je remplacer mon périphérique Direct3D 9 par le périphérique Direct3D 11, le contexte de périphérique, ou les deux ?


Le périphérique Direct3D est désormais utilisé pour créer des ressources dans la mémoire vidéo, alors que le contexte de périphérique sert à définir l’état du pipeline et générer des commandes de rendu. Pour plus d’informations, voir : [Quelles modifications majeures ont été apportées depuis la version Direct3D 9 ?](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>Dois-je mettre à jour le minuteur de mon jeu pour UWP ?


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904), de même que [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905), reste le meilleur moyen d’implémenter un minuteur de jeu pour les applications UWP.

Toutefois, on observe une nuance avec les minuteurs et le cycle de vie des applications UWP. Les commandes suspendre/reprendre sont différentes pour un joueur qui relance un jeu de bureau, car votre jeu reprendra sur une capture instantanée de la dernière fois que le jeu a été exécuté. Si le laps de temps avant la reprise est important, quelques semaines, par exemple, certaines implémentations du minuteur de jeu peuvent ne pas se comporter de façon fluide. Vous pouvez utiliser les événements de cycle de vie de l’application pour réinitialiser votre minuteur au moment de la reprise du jeu.

Les jeux qui utilisent encore l’instruction RDTSC doivent être mis à jour. Voir [Minutage des jeux et processeurs multicœurs](https://msdn.microsoft.com/library/windows/desktop/ee417693).

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>Le code de mon jeu est basé sur D3DX et DXUT. Existe-t-il des ressources pour m’aider à migrer mon code ?


Le projet de la communauté [Kit de ressources DirectX (DirectXTK)](http://go.microsoft.com/fwlink/p/?LinkID=248929) propose des classes d’assistance pour Direct3D 11.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>Comment maintenir les chemins de code pour le bureau et du Microsoft Store?


Série d’articles de Chuck intitulé [Techniques de codage à double usage pour les jeux](http://go.microsoft.com/fwlink/p/?LinkID=286210) offre des conseils sur le partage de code entre le bureau et les chemins de code Microsoft Store.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>Comment charger des ressources d’image dans mon jeu UWP DirectX ?


Il existe deux chemins d’API pour le chargement d’image :

-   Le pipeline de contenu convertit les images en fichiers DDS utilisés comme ressources de texture Direct3D. Voir [Utilisation de composants 3D dans votre jeu ou application](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).
-   Le [composant Imagerie Windows](https://msdn.microsoft.com/library/windows/desktop/ee719902) peut servir à charger des images de plusieurs formats et convient aussi bien aux bitmaps Direct2D qu’aux ressources de texture Direct3D.

Vous pouvez aussi utiliser DDSTextureLoader et WICTextureLoader, à partir du [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) ou [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926).

## <a name="where-is-the-directx-sdk"></a>Où est le Kit de développement logiciel (SDK) DirectX ?


Le Kit de développement logiciel (SDK) DirectX est inclus dans le Kit de développement logiciel (SDK) Windows. Le dernier kit de développement logiciel DirectX indépendant de celui de Windows date de juin 2010. Des exemples Direct3D figurent dans la bibliothèque de code avec les autres exemples d’applications Windows.

## <a name="what-about-directx-redistributables"></a>Qu’en est-il des redistribuables DirectX?


La grande majorité des composants du Kit de développement logiciel (SDK) Windows sont déjà inclus dans les versions prises en charge du système d’exploitation, ou ne comprennent pas de composant DLL (comme DirectXMath). Tous les composants API Direct3D utilisables par les applications UWP seront déjà disponibles pour votre jeu. Vous n’avez pas besoin de les redistribuer.

Les applications de bureau Win32 utilisent encore DirectSetup, donc si vous mettez aussi à jour la version de bureau de votre jeu, voir [Déploiement Direct3D 11 pour les développeurs de jeu](https://msdn.microsoft.com/library/windows/desktop/ee416644).

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>Existe-t-il un moyen de mettre à jour mon code d’application de bureau vers DirectX 11 avant de quitter Effects ?


Voir [Mise à jour d’Effects pour Direct3D 11](http://go.microsoft.com/fwlink/p/?LinkId=271568). Effects 11 vous aide à supprimer les dépendances sur les en-têtes hérités du kit de développement logiciel DirectX. Son rôle est d’aider au portage et il ne peut être utilisé qu’avec les applications de bureau.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>Existe-t-il un chemin d’accès pour porter mon jeu DirectX 8 vers UWP ?


Oui :

-   Voir [Conversion vers Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb204851).
-   Assurez-vous que votre jeu ne contient aucune bribe du pipeline corrigé. voir [Fonctionnalités déconseillées](https://msdn.microsoft.com/library/windows/desktop/cc308047).
-   Ensuite, suivez le chemin de portage DirectX 9 : [Effectuer le portage de D3D 9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>Puis-je porter mon jeu DirectX 10 ou 11 vers UWP ?


Les jeux de bureau DirectX 10.x et 11 sont faciles à porter vers UWP. Voir [Migration vers Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190).

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>Comment choisir le périphérique d’affichage dans un système multimoniteur ?


L’utilisateur sélectionne le moniteur sur lequel afficher votre application. Laissez Windows fournir l’adaptateur approprié en appelant [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) avec le premier paramètre défini sur **nullptr**. Ensuite, obtenez l’interface [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527) du périphérique, appelez [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) et utilisez la carte DXGI pour créer la chaîne d’échange.

## <a name="how-do-i-turn-on-antialiasing"></a>Comment activer l’anticrénelage?


L’anticrénelage (échantillonnage multiple) est activé quand vous créez le périphérique Direct3D. Énumérez la prise en charge de l’échantillonnage multiple en appelant [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499), puis définissez les options d’échantillonnage multiple dans la structure [**DXGI\_SAMPLE\_DESC structure**](https://msdn.microsoft.com/library/windows/desktop/bb173072) quand vous appelez [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530).

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>Le rendu de mon jeu s’appuie sur le multithreading et/ou le rendu différé. Que dois savoir pour Direct3D 11 ?


Consultez [Introduction au multithreading dans Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476891) pour la prise en main. Pour obtenir une liste des différences notables, voir [Différences de thread entre les versions Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476890). Notez que le rendu différé utilise un *contexte différé* d’appareil au lieu d’un *contexte immédiat*.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Où trouver de la documentation sur le pipeline programmable à partir de Direct3D 9?


Consultez les rubriques suivantes :

-   [Guide de programmation pour HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Direct3D 10 : Forum Aux Questions](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>Quel autre format de fichier puis-je utiliser pour remplacer le format .x de mes modèles ?


Il n’existe pas de format de remplacement officiel pour les fichiers .x, mais bon nombre des exemples utilisent le format SDKMesh. Visual Studio comprend aussi un [pipeline de contenu](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx) qui compile plusieurs formats courants en fichiers CMO. Ces fichiers peuvent être chargés avec du code à partir du starter kit 3D de Visual Studio, ou à l’aide du [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929).

## <a name="how-do-i-debug-my-shaders"></a>Comment déboguer mes nuanceurs ?


Studio2015 Visual Microsoft inclut des outils de diagnostic pour les graphiques DirectX. Voir [Débogage des graphiques DirectX](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>Quel est l’équivalent Direct3D 11 de la fonction *x* ?


Voir le [mappage des fonctions](feature-mapping.md#function-mapping) de la rubrique Mapper les fonctionnalités DirectX 9 aux API DirectX 11.

##  <a name="what-is-the-dxgiformat-equivalent-of-y-surface-format"></a>Quel est l’équivalent DXGI_FORMAT du format de surface *y* ?


Voir le [mappage des formats de surface](feature-mapping.md#surface-format-mapping) de la rubrique Mapper les fonctionnalités DirectX 9 aux API DirectX 11.

 

 




