---
author: Xansky
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Découvrez comment gérer les erreurs générées par la classe AdControl dans les bibliothèques de publicités Microsoft.
title: Gérer des erreurs dans les publicités
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows10, uwp, annonces, publicités, gestion des erreurs, javascript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: a9fa05ed2ee946fcec9ffb5ff21abd9011db0f2a
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7427684"
---
# <a name="handle-ad-errors"></a>Gérer des erreurs dans les publicités

Les classes [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) ont chacune un événement **ErrorOccurred** qui se déclenche en cas d'erreur liée aux publicités. Le code de votre application peut gérer cet événement et examiner les propriétés [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) et [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) de l’objet arguments d’événement pour déterminer la cause de l’erreur.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>Applications XAML

Pour gérer les erreurs liées aux publicités dans une application XAML:

1. Affectez l'événement **ErrorOccurred** de votre objet **AdControl**, **InterstitialAd** ou **NativeAdsManagerV2** au nom d’un délégué de gestionnaire d’événements.

2. Codez le délégué de gestion des événements d’erreur afin qu’il prenne deuxparamètres: un **Object** pour l’expéditeur et un objet [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs).

Voici un exemple qui affecte un délégué nommé **OnAdError** à l'événement **ErrorOccurred** d’un objet **AdControl** nommé *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Voici un exemple de définition du délégué **OnAdError** qui écrit des informations d’erreur dans la fenêtre Sortie de VisualStudio.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en XAML et C#.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>Applications JavaScript/HTML

Pour gérer les erreurs **ErrorOccur** dans une application JavaScript:

1.  Affectez l’événement **onErrorOccurred** à un gestionnaire d’événements.

2.  Codez le gestionnaire d’événements.

Voici un exemple qui affecte un gestionnaire d'événement nommé **errorLogger** à l'événement **ErrorOccurred** d'un object **AdControl**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

La fonction de gestion des erreurs est déclarative et doit être incorporée à la fonction [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx).

Le gestionnaire d’erreurs intercepte l’objet erreur JavaScript quand une erreur se produit. Cet objet fournit deuxarguments au gestionnaire d’erreurs. Pour plus d’informations, voir [Propriétés d’erreur particulières des méthodes Windows Runtime asynchrones](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx).

Voici un exemple de fonction de gestion des erreurs nommé **errorLogger** qui gère l’événement **onErrorOccurred**.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

Voir [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en JavaScript.
