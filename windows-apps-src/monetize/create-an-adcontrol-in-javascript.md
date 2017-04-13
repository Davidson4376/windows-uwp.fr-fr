---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "Découvrez comment créer par programmation un contrôle **AdControl** en JavaScript."
title: "Créer un AdControl en JavaScript"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, annonces, publicité, AdControl, javascript"
ms.openlocfilehash: b669925c3b630ddbfe82086231c46c951072244b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-an-adcontrol-in-javascript"></a>Créer un AdControl en JavaScript




Les exemples fournis dans cet article montrent comment créer par programmation un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en JavaScript. Cet article part du principe que vous avez déjà ajouté les références nécessaires dans votre projet pour pouvoir utiliser un **AdControl**. Pour obtenir plus d’informations, y compris une procédure pas à pas pour créer et initialiser un **AdControl** dans le balisage HTML au lieu de JavaScript, consultez [AdControl en HTML5 et Javascript](adcontrol-in-html-5-and-javascript.md).

## <a name="html-div-for-an-adcontrol"></a>Élément div HTML pour un AdControl

Un **AdControl** doit disposer d’un élément **div** dans la page HTML qui affiche la publicité. Le code indiqué ci-dessous fournit un exemple d’élément **div**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>JavaScript pour la création d’un AdControl

L’exemple suivant part du principe que vous utilisez un élément **div** existant dans votre code HTML avec l’ID **MyAd**.

Instanciez le **AdControl** dans la fonction **app.onactivated**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

Cet exemple suppose que vous avez déjà déclaré les méthodes de gestionnaires d’événements **myAdError**, **myAdRefreshed** et **myAdEngagedChanged**.

>**Remarque**&nbsp;&nbsp;Les valeurs *applicationId* et *adUnitId* indiquées dans cet exemple sont des [valeurs du mode test](test-mode-values.md). Vous devez [remplacer ces valeurs par les valeurs dynamiques](set-up-ad-units-in-your-app.md) du Centre de développement Windows avant de soumettre votre application.

Si vous utilisez ce code et que vous ne voyez pas de publicités, vous pouvez essayer d’insérer un attribut de **position:relative** dans l’élément **div** qui contient le **AdControl**. Cela remplace le paramètre par défaut de l’élément **IFrame**. Les publicités apparaissent correctement, sauf si elles ne sont pas affichées en raison de la valeur de cet attribut. Notez que les nouvelles unités publicitaires peuvent ne pas être disponibles pendant 30minutes.

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 

 
