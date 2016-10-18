---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "Découvrez comment affecter les propriétés **AdControl** aux valeurs."
title: "Exemple de propriétés XAML"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: fb0533aa0ea760bca686276f886f0afcb21bf6f7


---

# Exemple de propriétés XAML




L’exemple de code XAML suivant montre comment attribuer des propriétés [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) aux valeurs. Si une propriété n’est pas définie, le contrôle **AdControl** utilisera les valeurs par défaut pour créer une publicité cohérente avec l’expérience utilisateur de l’application.

Les valeurs indiquées sont des exemples. Dans votre code, vous allez définir les valeurs de ces fonctions et propriétés appropriées pour votre application.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


