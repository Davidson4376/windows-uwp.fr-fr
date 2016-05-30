---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Découvrez comment gérer les événements de la classe AdControl.
title: Événements AdControl en JavaScript

---

# Événements AdControl en JavaScript


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Les exemples suivants montrent comment gérer les événements de la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). Ces exemples reposent sur le principe que vous avez déjà affecté les gestionnaires d’événements aux événements **AdControl**. Pour plus d’informations sur la procédure à suivre pour ce faire, voir [Exemple de propriétés HTML](html-properties-example.md).

En JavaScript, les événements **AdControl** doivent être délimités par la fonction [MarkSupportedForProcessing](http://msdn.microsoft.com/en-us/library/windows/apps/Hh967819.aspx). Pour plus d’informations sur la gestion des événements en JavaScript, voir [Codage d’applications de base (HTML)](https://msdn.microsoft.com/en-us/library/windows/apps/hh780660.aspx#adding-event-handlers).

## Exemples

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Gestion des erreurs AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 


<!--HONumber=May16_HO2-->


