---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "Découvrez comment installer les bibliothèques de publicités Microsoft."
title: "Installer les bibliothèques de publicités Microsoft"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0951818ceaf3d96543f9f97ec6993d08fdaab2b8


---

# Installer les bibliothèques de publicités Microsoft


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Les bibliothèques de publicités Microsoft pour les applications Windows sont incluses dans le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft](http://aka.ms/store-em-sdk). Ce Kit de développement logiciel (SDK) est une extension de VisualStudio2015 et de Visual Studio2013.

Pour connaître les instructions d’installation, voir [Monétiser votre application et engager les clients avec le SDK d’engagement et de monétisation de la BoutiqueMicrosoft](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk).

> **Remarque** Si vous avez installé Windows10 Anniversary SDK Preview Build14295 ou ultérieure avec Visual Studio2015, vous devez également installer la bibliothèque WinJS si vous souhaitez ajouter des publicités à une application JavaScript/HTML. Cette bibliothèque était incluse dans les versions précédentes du Kit de développement logiciel Windows (Kit SDK Windows) pour Windows10, mais depuis Windows10 Anniversary SDK Preview Build14295, elle doit être installée séparément. Pour installer WinJS, voir [Obtenir WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## Noms des bibliothèques de publicités et de médiation publicitaire


Le Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft comprend deuxensembles de bibliothèques de publicités: les bibliothèques de publicités Microsoft (qui fournissent les classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) et [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour les applications XAML et JavaScript/HTML) et les bibliothèques de médiation publicitaire (qui fournissent la classe **AdMediatorControl**).

Cette documentation décrit l’utilisation des classes **AdControl** et **InterstitialAd** des bibliothèques de publicités Microsoft pour afficher des bannières et des spots vidéo publicitaires de Microsoft. Pour plus d’informations sur l’utilisation de la médiation publicitaire, voir [Utiliser la médiation publicitaire pour optimiser les revenus](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue).


Pour pouvoir utiliser les contrôles de publicité dans le code de votre application, vous devez référencer la bibliothèque appropriée dans votre projet. Le tableau suivant répertorie les noms de chacune des bibliothèques du Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft, tels qu’ils apparaissent dans la boîte de dialogue **Gestionnaire de références** de VisualStudio.


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

 

 

 



<!--HONumber=Jun16_HO4-->


