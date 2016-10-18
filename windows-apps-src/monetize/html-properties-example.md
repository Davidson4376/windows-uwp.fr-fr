---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "Découvrez comment affecter les propriétés **AdControl** au format HTML."
title: "Exemple de propriétés HTML"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 1898ed2ccad74ac33c5130c627363e0a9daebceb


---

# Exemple de propriétés HTML




L’exemple suivant montre comment affecter les propriétés [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) au format HTML. Les propriétés **applicationId** et **adUnitId** sont requises. Les autres propriétés et événements sont facultatifs. Si vous n’indiquez pas les valeurs des propriétés facultatives, le contrôle utilise les valeurs par défaut qui créent une publicité en harmonie avec l’expérience utilisateur de l’application.

Les cinqdernières lignes sont un exemple d’inscription des fonctions dans les événements **AdControl**. Vous ne pouvez inscrire que les fonctions que vous avez définies dans votre code JavaScript.

Les valeurs indiquées sont des exemples. Dans votre code, vous allez définir les valeurs de ces fonctions et propriétés pour qu’elles conviennent à votre application.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


