---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "Découvrez comment affecter les propriétés **AdControl** au format HTML."
title: "Exemple de propriétés AdControl HTML"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, annonces publicitaires, publicité, AdControl, HTML, propriétés"
ms.openlocfilehash: efc94b723b3bfe97b35484882ae784e9ef4d6807
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-html-properties-example"></a>Exemple de propriétés AdControl HTML

L’exemple suivant montre comment affecter les propriétés [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) au format HTML. Les propriétés **applicationId** et **adUnitId** sont requises. Les autres propriétés et événements sont facultatifs. Si vous n’indiquez pas les valeurs des propriétés facultatives, le contrôle utilise les valeurs par défaut qui créent une publicité en harmonie avec l’expérience utilisateur de l’application.

Les cinqdernières lignes sont un exemple d’inscription des fonctions dans les événements **AdControl**. Vous ne pouvez inscrire que les fonctions que vous avez définies dans votre code JavaScript.

Les valeurs indiquées sont des exemples. Dans votre code, vous allez définir les valeurs de ces fonctions et propriétés pour qu’elles conviennent à votre application.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl"
    data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                       adUnitId: '10865270',
                       isAutoRefreshEnabled: false,
                       onAdRefreshed: myAdRefreshed,
                       onErrorOccurred: myAdError,
                       onEngagedChanged: myAdEngagedChanged,
                       onPointerDown: myPointerDown,
                       onPointerUp: myPointerUp }" />
```

## <a name="related-topics"></a>Rubriques connexes

* [AdControl en HTML5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 
