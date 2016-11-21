---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "Découvrez comment installer les bibliothèques de publicités Microsoft."
title: "Installer les bibliothèques de publicités Microsoft"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: c717fa693c6edf8757c3eef79d60193434104bd8


---

# Installer les bibliothèques de publicités Microsoft




Pour les applications de plateforme Windows universelle (UWP) pour Windows10, les bibliothèques de publicités Microsoft sont incluses dans [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Ce kit de développement logiciel(SDK) est une extension de VisualStudio2015 et des versions ultérieures. Pour plus d’informations sur l’installation de ce SDK, consultez [cet article](microsoft-store-services-sdk.md).

> **Remarque**&nbsp;&nbsp;Si vous avez installé le SDK Windows10 (14393) ou version ultérieure, vous devez également installer la bibliothèque WinJS si vous voulez ajouter des publicités à une application UWP JavaScript/HTML. Cette bibliothèque était incluse dans les versions précédentes du SDK Windows10, mais à partir du SDK Windows10 (14393), vous devez l’installer séparément. Pour installer WinJS, voir [Obtenir WinJS](http://try.buildwinjs.com/download/GetWinJS/).

Pour les applications XAML et JavaScript/HTML pour Windows8.1 et Windows Phone8.x, les bibliothèques de publicités Microsoft sont incluses dans [Microsoft Advertising SDK pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk). Ce kit de développement logiciel (SDK) est une extension de VisualStudio2015 et de Visual Studio2013.

Pour les applications Silverlight Windows Phone8.x, les bibliothèques de publicités Microsoft sont disponibles dans un package NuGet que vous pouvez télécharger et installer dans votre projet. Pour plus d’informations, voir [AdControl dans Silverlight WindowsPhone](adcontrol-in-windows-phone-silverlight.md).

## Noms des bibliothèques de publicités


Il existe plusieurs bibliothèques de publicités différentes disponibles dans Microsoft Store Services SDK et Microsoft Advertising SDK pour Windows et Windows Phone8.x:

* Microsoft Store Services SDK inclut les bibliothèques de publicités Microsoft (qui fournissent les classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) et [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour les applications XAML et JavaScript/HTML).

* Microsoft Advertising SDK pour Windows et Windows Phone8.x comprend deuxensembles de bibliothèques de publicités: les bibliothèques de publicités Microsoft (qui fournissent les classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) et [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour les applications XAML et JavaScript/HTML) et les bibliothèques de médiation publicitaire (qui fournissent la classe **AdMediatorControl**).

Cette documentation décrit l’utilisation des classes **AdControl** et **InterstitialAd** des bibliothèques de publicités Microsoft pour afficher des bannières et des spots vidéo publicitaires. Pour plus d’informations sur l’utilisation de la médiation publicitaire pour les applications Windows8.1 et Windows Phone8.x, voir [Utiliser la médiation publicitaire pour optimiser les revenus](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Remarque**&nbsp;&nbsp;La médiation publicitaire à l’aide de la classe **AdMediatorControl** n’est actuellement pas prise en charge pour les applications UWP pour Windows10. La médiation côté serveur sera disponible très prochainement pour les applications UWP à l’aide des API dédiées aux bannières publicitaires (**AdControl**) et aux spots vidéo publicitaires (**InterstitialAd**).

Pour pouvoir utiliser les contrôles de publicité dans le code de votre application, vous devez référencer la bibliothèque appropriée dans votre projet. Le tableau suivant répertorie les noms de chacune des bibliothèques comme ils figurent dans la boîte de dialogue **Gestionnaire de références** de Visual Studio.


<table>
    <thead>
        <tr><th>Nom du contrôle</th><th>Type de projet</th><th>Nom de la bibliothèque dans le gestionnaire de références</th><th>Numéro de version</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** et **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>Kit de développement logiciel (SDK) MicrosoftAdvertising pour XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone8.1</td>
            <td>Kit de développement logiciel (SDK) Ad Mediator pour WindowsPhone8.1XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** et **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>Kit de développement logiciel (SDK) MicrosoftAdvertising pour JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>Kit de développement logiciel (SDK) MicrosoftAdvertising pour Windows8.1 natif (JS)</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone8.1</td>
            <td>Kit de développement logiciel (SDK) MicrosoftAdvertising pour WindowsPhone8.1 natif (JS)</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl** (XAML uniquement)</td>
            <td>UWP</td>
            <td>Kit de développement logiciel (SDK) MicrosoftAdvertisingUniversal</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone8.1</td>
            <td>Kit de développement logiciel (SDK) Ad Mediator pour WindowsPhone8.1XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Nov16_HO1-->


