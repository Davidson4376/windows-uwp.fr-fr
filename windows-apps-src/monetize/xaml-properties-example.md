---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "Découvrez comment affecter les propriétés **AdControl** aux valeurs."
title: "Exemple de propriétés AdControl XAML"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, publicités, publicité, AdControl, XAML, propriétés"
ms.openlocfilehash: 3c5dae93ab6ee58fac7d4593257d357f268c241a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-xaml-properties-example"></a>Exemple de propriétés AdControl XAML

L’exemple de code XAML suivant montre comment attribuer des propriétés [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) aux valeurs. Si une propriété n’est pas définie, le contrôle **AdControl** utilisera les valeurs par défaut pour créer une publicité cohérente avec l’expérience utilisateur de l’application.

Les valeurs indiquées sont des exemples. Dans votre code, vous allez définir les valeurs de ces fonctions et propriétés appropriées pour votre application.

> [!div class="tabbedCodeSnippets"]
``` xml
<UI:AdControl Width="300",
    Height="250",
    AdUnitId="10865270",
    ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
    IsAutoRefreshEnabled="false",
    AdRefreshed="OnAdRefresh",
    ErrorOcurred="OnAdError",
    IsEngagedChanged="OnAdEngagedChanged" />
```

## <a name="related-topics"></a>Rubriques connexes

* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 
