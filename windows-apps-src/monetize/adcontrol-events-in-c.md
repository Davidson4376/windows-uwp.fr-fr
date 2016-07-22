---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "Découvrez comment gérer les événements de la classe AdControl."
title: "Événements AdControl en C#"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: f92cbbb00a064ce7569d44ad952838df4d21ac8c

---

# Événements AdControl en C\# #  


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Les exemples suivants montrent comment gérer les événements de la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). Ces exemples reposent sur le principe que vous avez déjà affecté les gestionnaires d’événements aux événements **AdControl** en XAML. Pour plus d’informations sur la procédure à suivre pour ce faire, voir [Exemple de propriétés XAML](xaml-properties-example.md).

Pour plus d’informations sur la gestion des événements en C#, voir [Vue d’ensemble des événements et des événements routés (applications Windows universelles en C#/VB/C++ et XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## Exemples


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Gestion des erreurs AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Jul16_HO2-->


