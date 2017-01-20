---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Apprenez à utiliser la classe AdControl pour afficher des bannières pub. dans une app. HTML/JavaScript pour Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1."
title: "AdControl en HTML 5 et JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 509cfe00050c5b5b3997af0e2906676f946d9278

---

# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 et JavaScript

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) pour afficher des bannières publicitaires dans une application HTML/JavaScript pour Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1. Cette procédure pas à pas n’utilise ni **AdMediatorControl** ni la médiation publicitaire.

Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application HTML/JavaScript, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Conditions préalables


* Pour les applications UWP : installez [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) avec Visual Studio 2015.
* Pour les applications Windows 8.1 ou Windows Phone 8.1 : [installez le Kit de développement logiciel (SDK) Microsoft Advertising pour Windows et Windows Phone 8.x](http://aka.ms/store-8-sdk) avec Visual Studio 2015 ou Visual Studio 2013.

> **Remarque**&nbsp;&nbsp;Si vous avez installé Windows 10 Anniversary SDK Preview Build 14295 ou ultérieure avec Visual Studio 2015, vous devez aussi installer la bibliothèque WinJS. Cette bibliothèque était incluse dans les versions précédentes du Kit de développement logiciel Windows (Kit SDK Windows) pour Windows 10, mais depuis Windows 10 Anniversary SDK Preview Build 14295, elle doit être installée séparément. Pour installer WinJS, voir [Obtenir WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## <a name="code-development"></a>Développement du code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**

4.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet :

    -   Pour un projet de plateforme Windows universelle (UWP) : développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).

    -   Pour un projet Windows 8.1 : développez **Windows 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows 8.1 natif (JS)**.

    -   Pour un projet Windows Phone 8.1 : développez **Windows Phone 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows Phone 8.1 natif (JS)**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **Remarque**&nbsp;&nbsp;Cette image correspond à la génération d’un projet UWP pour Windows 10 à l’aide de Visual Studio 2015. Si vous générez une application Windows 8.1 ou Windows Phone 8.1 à l’aide de Visual Studio 2013, votre écran aura une apparence différente.

5.  Dans **Gestionnaire de références**, cliquez sur OK.

6.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet).

7.  Dans la section **&lt;head&gt;**, après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js.

  Dans un projet UWP, ajoutez le code suivant.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Dans un projet Windows 8.1 ou Windows Phone 8.1, ajoutez le code suivant.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

  <span/>
  >**Remarque**&nbsp;&nbsp;Vous devez placer cette ligne dans la section **&lt;head&gt;** après l’inclusion de main.js, faute de quoi vous rencontrerez une erreur pendant la génération de votre projet. Si votre projet cible Windows 8.1 ou Windows Phone 8.1, le fichier HTML par défaut dans votre projet s’appelle default.html au lieu de index.html et le fichier JavaScript par défaut dans votre projet s’appelle default.js au lieu de main.js.

8.  Modifiez la section **&lt;body&gt;** dans le fichier default.html (ou un autre fichier html, selon votre projet) pour inclure l’élément div correspondant à la classe **AdControl**. Affectez les propriétés **applicationId** et **adUnitId** de la classe **AdControl** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md), puis ajustez la hauteur et la largeur du contrôle en tenant compte des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

  > **Remarque**&nbsp;&nbsp;Avant de soumettre votre application, remplacez les valeurs de test **applicationId** et **adUnitId** par des valeurs dynamiques.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
  </div>
  ```

9.  Compilez et exécutez l’application pour la voir avec une publicité.

## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publier l’application avec des publicités dynamiques via le Centre de développement Windows


1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation** &gt; **Monétiser avec des publicités** de votre application, puis [créez une unité Microsoft Advertising autonome](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**applicationId** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

## <a name="complete-indexhtml-for-a-sample-uwp-project"></a>Fichier index.html complet correspondant à un exemple de projet UWP

> [!div class="tabbedCodeSnippets"]
``` html
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
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
 

 



<!--HONumber=Dec16_HO2-->


