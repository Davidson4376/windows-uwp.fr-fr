---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: "Découvrez comment gérer les erreurs générées par la classe AdControl dans les bibliothèques de publicités Microsoft."
title: "Gestion des erreurs liées aux bibliothèques de publicités Microsoft"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: dedac33d86f50b63de300f78a9f9961efc1c016b

---

# Gestion des erreurs liées aux bibliothèques de publicités Microsoft




Cette rubrique fournit des informations de base sur la gestion des erreurs générées par la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) dans les bibliothèques de publicités Microsoft.

<span id="bkmk-javascript"/>
## Applications JavaScript/HTML

Pour gérer les erreurs **AdControl** dans une application JavaScript:

1.  Affectez l’événement **onErrorOccurred** à un gestionnaire d’événements.

2.  Codez le gestionnaire d’événements.

Le gestionnaire d’événements **onErrorOccurred** est défini dans l’attribut **data-win-options** de l’élément **div** de l’objet **AdControl**. Dans l’exemple suivant, l’événement **onErrorOccurred** est défini pour être géré par une fonction nommée **errorLogger**.

``` syntax
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

La fonction de gestion des erreurs est déclarative et doit être incorporée à la fonction [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx).

Le gestionnaire d’erreurs intercepte l’objet erreur JavaScript quand une erreur **AdControl** se produit. Cet objet fournit deuxarguments au gestionnaire d’erreurs. Pour plus d’informations, voir [Propriétés d’erreur particulières des méthodes Windows Runtime asynchrones](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx).

Voici un exemple de fonction de gestion des erreurs nommé **errorLogger** qui gère l’événement **onErrorOccurred**.

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage + " error code: " + evt.errorCode + \n");
});
```

Voir [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en JavaScript.

<span id="bkmk-dotnet"/>
## Applications XAML

Pour gérer les erreurs **AdControl** dans une application XAML:

* Affectez l’événement **ErrorOccurred** au nom d’un délégué de gestionnaire d’événements.

* Codez le délégué de gestion des événements d’erreur afin qu’il prenne deuxparamètres: un **Object** pour l’expéditeur et un objet **AdErrorEventArgs**.

Voici un exemple qui affecte un délégué nommé **OnAdError** à l’événement **ErrorOccurred**.

``` syntax
this.ErrorOccurred = OnAdError;
```

Voici un exemple de définition du délégué **OnAdError** qui écrit des informations d’erreur dans la fenêtre Sortie de VisualStudio.

``` syntax
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
}
```

Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en XAML et C#.

 

 



<!--HONumber=Aug16_HO3-->


