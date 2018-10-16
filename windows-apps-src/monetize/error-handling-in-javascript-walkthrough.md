---
author: Xansky
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Découvrez comment intercepter les erreurs AdControl dans votre application.
title: Gestion des erreurs dans la procédure pas à pas pour JavaScript
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, annonces publicitaires, publicité, gestion des erreurs, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 5e25de40c7fd28cb43c308bd0361b400e7bf6909
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4619831"
---
# <a name="error-handling-in-javascript-walkthrough"></a>Procédure pas à pas pour la gestion des erreurs dans JavaScript

Cette procédure pas à pas montre comment intercepter les erreurs liées aux publicités dans votre application JavaScript. Cette procédure pas à pas utilise un [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher une bannière, mais les concepts généraux s’appliquent également aux spots publicitaires et aux publicités natives.

Ces exemples partent du principe que vous disposez d’une application JavaScript qui contient un **AdControl**. Pour obtenir des instructions pas à pas qui montrent comment ajouter un **AdControl** à votre application, voir [AdControl en HTML5 JavaScript](adcontrol-in-html-5-and-javascript.md). Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application HTML/JavaScript, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

1.  Dans le fichier default.html, ajoutez une valeur pour l’événement **onErrorOccurred** où vous définissez les options **data-win-options** dans l’élément **div** du contrôle **AdControl**. Recherchez le code ci-dessous dans le fichier default.html.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Après l’attribut **adUnitId**, ajoutez la valeur de l’événement **onErrorOccurred**.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Créez un élément **div** qui affiche le texte afin de pouvoir consulter les messages générés. Pour ce faire, ajoutez le code suivant après l’élément **div** de **myAd**.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Créez un **AdControl** pour déclencher un événement d’erreur. Il ne peut exister qu’un ID d’application pour tous les objets **AdControl** d’une application. Par conséquent, la création d’une application supplémentaire avec un autre ID d’application déclenche une erreur au moment de l’exécution. Pour ce faire, après les sections **div** précédentes que vous avez ajoutées, ajoutez le code suivant au corps de la page default.html.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  Dans le fichier default.js du projet, après la fonction d’initialisation par défaut, vous allez ajouter le gestionnaire d’événements pour **errorLogger**. Faites défiler le fichier jusqu’en bas, puis après le dernier point-virgule, insérez le code suivant.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Générez et exécutez le fichier. Vous pouvez voir la publicité d’origine de l’exemple d’application généré précédemment ainsi que le texte figurant sous cette publicité qui décrit l’erreur. Vous ne voyez pas la publicité dotée de l’ID de publicité **liveAd**.

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
