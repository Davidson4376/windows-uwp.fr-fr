---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Store Services SDK vous offre différentes manières de monétiser votre application avec des publicités."
title: "Afficher des publicités dans votre application"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, annonces publicitaires, publicité, bannière, spot publicitaire"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b6343d8a011a3e62a3b714c7dab280c9d9a8f81d
ms.lasthandoff: 02/07/2017

---

# <a name="display-ads-in-your-app"></a>Afficher des publicités dans votre application


La plateforme Windows universelle (UWP) et le Windows Store offrent plusieurs façons de monétiser votre application à l’aide de publicités.

## <a name="display-banner-and-video-interstitial-ads-using-the-microsoft-advertising-libraries"></a>Afficher des bannières et des spots vidéo publicitaires à l’aide des bibliothèques de publicités Microsoft

Gagnez plus d’argent avec vos applications UWP et vos applications pour Windows 8.1 et Windows Phone 8.x en incluant des bannières et des spots vidéo publicitaires. Les publicités s’affichent dans les applications Windows pour PC, tablettes et téléphones. Vous pouvez surveiller les performances des publicités en temps réel à l’aide du [rapport sur les performances publicitaires](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement Windows.

Pour inclure ces types de publicités dans vos applications, utilisez les contrôles **AdControl** et **InterstitialAd** dans les bibliothèques de publicités distribuées dans [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (pour les applications UWP) et [Microsoft Advertising SDK pour Windows et Windows Phone 8.x](http://aka.ms/store-8-sdk) (pour les applications Windows 8.1 et Windows Phone 8.x).


Les rubriques suivantes fournissent des informations sur les tâches courantes impliquant les bibliothèques de publicités Microsoft.

|  Tâche    | Rubrique |               
|----------|-------|
| Installer et utiliser les bibliothèques de publicités Microsoft.     | Voir [Prise en main des bibliothèques de publicités Microsoft](get-started-with-microsoft-advertising-libraries.md).        |
| Afficher des bannières publicitaires dans votre application XAML/C#.     | Voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md).        |
| Afficher des bannières publicitaires dans votre application HTML/JavaScript.     | Voir [AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md).        |
| Afficher des bannières publicitaires dans votre application Silverlight Windows Phone 8.x.     | Voir [AdControl dans Silverlight Windows Phone](adcontrol-in-windows-phone-silverlight.md).        |
| Présenter un spot publicitaire dans votre application.     | Voir [Spots publicitaires](interstitial-ads.md).       |
| Ajouter des publicités à des contenus vidéo dans une application de plateforme Windows universelle (UWP) écrite en JavaScript avec HTML.   |  Voir [Ajouter des publicités à des contenus vidéo en HTML 5 et JavaScript](add-advertisements-to-video-content.md).  |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     |Voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).       |
| Gérer les erreurs [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) dans votre application.     | Voir [Gestion des erreurs](error-handling-with-advertising-libraries.md) ainsi que les procédures pas à pas dans [Gestion des erreurs AdControl](adcontrol-error-handling.md).       |
| Signaler un bogue dans les bibliothèques de publicités Microsoft.     | Consultez la [page de support](https://go.microsoft.com/fwlink/p/?LinkId=331508).        |
| Bénéficier du support de la communauté.     | Consultez le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |

                            

## <a name="use-ad-mediation-for-banner-ads-windows-81-and-windows-phone-8x"></a>Utiliser la médiation publicitaire pour les bannières publicitaires (Windows 8.1 et Windows Phone 8.x)

Pour les applications Windows 8.1 et Windows Phone 8.x, vous pouvez utiliser la classe **AdMediatorControl** pour optimiser vos recettes publicitaires en affichant des bannières publicitaires provenant de plusieurs réseaux publicitaires. Une fois que vous avez ajouté ce contrôle à votre application, vous pouvez configurer vos paramètres de médiation publicitaire dans le tableau de bord du Centre de développement Windows, et nous nous chargeons de la médiation des demandes de bannières publicitaires des réseaux publicitaires que vous choisissez. Pour plus d’informations, voir [Utiliser la médiation publicitaire pour optimiser les recettes publicitaires](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Remarque**&nbsp;&nbsp;La médiation publicitaire à l’aide de la classe **AdMediatorControl** n’est actuellement pas prise en charge pour les applications UWP pour Windows 10. La médiation côté serveur sera disponible très prochainement pour les applications UWP à l’aide des API dédiées aux bannières publicitaires (**AdControl**) et aux spots vidéo publicitaires (**InterstitialAd**). Pour obtenir des instructions sur la migration depuis **AdMediatorControl** vers **AdControl** dans votre application UWP, voir [Migration d’AdMediatorControl vers AdControl pour les applications UWP](migrate-from-admediatorcontrol-to-adcontrol.md).

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Prise en charge des publicités pour les projets Silverlight Windows Phone 8.x

Certains scénarios de développement ne sont plus pris en charge dans les projets Silverlight Windows Phone 8.x. Pour plus d’informations, voir le tableau suivant.

|  Version de la plateforme  |  Projets existants    |   Nouveaux projets  |
|-----------------|----------------|--------------|
| Silverlight Windows Phone 8.0     |  Si vous avez un projet Silverlight Windows Phone 8.0 existant qui utilise déjà un **AdControl** ou **AdMediatorControl** d’une précédente version de Microsoft Universal Ad Client SDK ou Microsoft Advertising SDK et que cette application est déjà publiée dans le Windows Store, vous pouvez modifier, puis régénérer le projet, puis vous pouvez déboguer ou tester vos modifications sur un appareil. Le débogage ou les tests du projet dans l’émulateur ne sont pas pris en charge.  |  Non pris en charge.  |
| Silverlight Windows Phone 8.1    |  Si vous disposez d’un projet Silverlight Windows Phone 8.1 qui utilise un **AdControl** ou **AdMediatorControl** d’un kit de développement logiciel (SDK) antérieur, vous pouvez modifier et régénérer le projet. Toutefois, pour déboguer ou tester l’application, vous devez l’exécuter dans l’émulateur et utiliser des [valeurs de mode de test](test-mode-values.md) pour les ID d’application et ID d’unité publicitaire. Le débogage ou les tests de l’application sur un appareil ne sont pas pris en charge.  |   Vous pouvez ajouter un **AdControl** ou **AdMediatorControl** à un nouveau projet Silverlight Windows Phone 8.1. Toutefois, pour déboguer ou tester l’application, vous devez l’exécuter dans l’émulateur et utiliser des [valeurs de mode de test](test-mode-values.md) pour les ID d’application et ID d’unité publicitaire. Le débogage ou les tests de l’application sur un appareil ne sont pas pris en charge. |

## <a name="related-topics"></a>Rubriques connexes

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [Monétiser votre application en insérant des annonces publicitaires](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)

