---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "Découvrez comment affecter les propriétés **AdControl** aux valeurs."
title: "Exemple de propriétés AdControl XAML"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: f33e4048a1a9aa68ecc627d81ca15858027720c1

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

 



<!--HONumber=Dec16_HO2-->


