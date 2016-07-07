---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "Découvrez comment créer par programme un contrôle **AdControl** en JavaScript."
title: "Créer un AdControl en JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 481f9d785181ca197debdb807bb0b0c7b4168632


---

# Créer un AdControl en JavaScript


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cet exemple montre comment créer par programme un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en JavaScript.

## Élément div HTML pour un AdControl

Un **AdControl** doit disposer d’un élément **div** dans la page HTML qui affiche la publicité. Le code indiqué ci-dessous fournit un exemple d’élément **div**.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## JavaScript pour la création d’un AdControl

L’exemple suivant part du principe que vous utilisez un élément **div** existant dans votre code HTML avec l’ID **MyAd**.

Instanciez le **AdControl** dans la fonction **app.onactivated**.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

Les valeurs indiquées sont des exemples. Dans votre code, vous allez définir les valeurs de ces fonctions et propriétés appropriées pour votre application.

Si vous utilisez ce code et que vous ne voyez pas de publicités, vous pouvez essayer d’insérer un attribut de **position:relative** dans l’élément **div** qui contient le **AdControl**. Cela remplace le paramètre par défaut de l’élément **IFrame**. Les publicités apparaissent correctement, sauf si elles ne sont pas affichées en raison de la valeur de cet attribut. Notez que les nouvelles unités publicitaires peuvent ne pas être disponibles pendant 30minutes.

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


