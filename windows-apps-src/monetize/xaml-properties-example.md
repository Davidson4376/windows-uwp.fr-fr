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
keywords: "Windows 10, uwp, publicités, publicité, AdControl, XAML, propriétés"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dbcc08b1373c7f73d5b9ebf541ec82bb01fd9df3
ms.lasthandoff: 02/07/2017

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

 

