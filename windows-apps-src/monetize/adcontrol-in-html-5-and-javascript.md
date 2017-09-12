---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Apprenez à utiliser la classe AdControl pour afficher des bannières pub. dans une app. HTML/JavaScript pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1."
title: AdControl en HTML5 et JavaScript
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, annonces publicitaires, publicité, AdControl, contrôle de publicité, javascript, HTML"
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML5 et JavaScript

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) pour afficher des bannières publicitaires dans une application HTML/JavaScript pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1.

Pour obtenir un exemple de projet complet présentant la méthode pour ajouter des bannières publicitaires à une application JavaScript/HTML, consultez les [exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Conditions préalables


* Pour les applications UWP: installez le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec VisualStudio2015 ou version ultérieure.
* Pour les applicationsWindows8.1 ou WindowsPhone8.1: [installe Microsoft Advertising SDK pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) avec Visual Studio2015 ou Visual Studio2013.

> [!NOTE]
> Si vous ajoutez des bannières publicitaires à une application UWP et que vous avez installé le SDK Windows10 version10.0.14393 (mise à jour anniversaire) ou une version ultérieure du SDK Windows, vous devez également installer la bibliothèque WinJS. Cette bibliothèque était incluse dans les versions précédentes du Kit de développement logiciel Windows (Kit SDK Windows) pour Windows10, mais depuis le SDK Windows10 version10.0.14393 (mise à jour anniversaire), elle doit être installée séparément. Pour installer WinJS, voir [Obtenir WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## <a name="code-development"></a>Développement du code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**

4.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows8.1 natif (JS)**.

    -   Pour un projet WindowsPhone8.1: développez **WindowsPhone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **SDK MicrosoftAdvertising pour WindowsPhone8.1 natif (JS)**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  Dans **Gestionnaire de références**, cliquez sur OK.

6.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet).

7.  Dans la section **&lt;head&gt;**, après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js.

    Dans un projet UWP, ajoutez le code suivant.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Dans un projet Windows8.1 ou WindowsPhone8.1, ajoutez le code suivant.

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > Vous devez placer cette ligne dans la section **&lt;head&gt;** après avoir inclus main.js. Sinon, vous risquez de rencontrer une erreur lors de la création de votre projet. Si votre projet cible Windows8.1 ou Windows Phone8.1, le fichier HTML par défaut dans votre projet s’appelle default.html au lieu de index.html et le fichier JavaScript par défaut dans votre projet s’appelle default.js au lieu de main.js.

8.  Modifiez la section **&lt;body&gt;** dans le fichier default.html (ou un autre fichier html, selon votre projet) pour inclure l’élément div correspondant à la classe **AdControl**. Affectez les propriétés **applicationId** et **adUnitId** de la classe **AdControl** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md). Ajustez également la hauteur et la largeur de la commande pour qu’elle corresponde à l’une des [tailles de publicités prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **AdControl** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compilez et exécutez l’application pour la voir avec une publicité.

L’exemple suivant montre le fichier index.html complet pour une application UWP simple.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publier votre application avec des publicités dynamiques via le Centre de développement Windows

1.  Dans le tableau de bord du Centre de développement, accédez à la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) de votre application, puis [créez une unité publicitaire](../monetize/set-up-ad-units-in-your-app.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID de l’unité de publicité et de l’ID de l’application.

2. Si votre application est de type UWP pour Windows10, vous pouvez éventuellement activer la médiation publicitaire pour la commande **AdControl** en configurant les paramètres dans la section [Médiation publicitaire](../publish/monetize-with-ads.md#mediation) sur la page [Monétiser avec des publicités](../publish/monetize-with-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

3.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**applicationId** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

4.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

5.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs objets **AdControl** dans une seule application (par exemple, chaque page de votre application peut héberger un objet **AdControl** différent). Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/monetize-with-ads.md#mediation) séparément et d'obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques associées

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Configurer des unités publicitaires pour votre application](../monetize/set-up-ad-units-in-your-app.md)
