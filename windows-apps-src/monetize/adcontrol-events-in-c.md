---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "Découvrez comment gérer les événements de la classe AdControl."
title: "Événements AdControl en C#"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, annonces, publicité, AdControl, événements"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: cf285ebb4207b9a9a215bfb4a739b0bc6a2d934b
ms.lasthandoff: 02/07/2017

---

# <a name="adcontrol-events-in-c"></a>Événements AdControl en C\# #  


Les exemples suivants illustrent les gestionnaires d’événements de base pour les événements [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) suivants : [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) et [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx). Ces exemples reposent sur le principe que vous avez déjà affecté les gestionnaires d’événements aux événements dans votre code XAML. Pour plus d’informations sur la procédure à suivre pour ce faire, voir [Exemple de propriétés XAML](xaml-properties-example.md).

Pour plus d’informations sur la gestion des événements en C#, voir [Vue d’ensemble des événements et des événements routés (applications Windows universelles en C#/VB/C++ et XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## <a name="examples"></a>Exemples

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Gestion des erreurs AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 

