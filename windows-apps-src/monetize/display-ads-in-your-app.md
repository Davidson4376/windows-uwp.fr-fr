---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Le SDK MicrosoftAdvertising vous offre plusieurs moyens de monétiser votre application grâce aux publicités."
title: "Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, publicités, publicité, bannière, contrôle de publicité, spot"
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising

Augmentez vos opportunités de revenus en plaçant des publicités dans vos applications à l’aide du SDK Microsoft Advertising. Notre plateforme de monétisation des publicités offre une grande diversité de types de publicités et prend en charge la médiation avec de nombreux réseaux publicitaires populaires.

Pour afficher des publicités dans vos applications, procédez comme suit.

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Étape1: Installer le SDK Microsoft Advertising

Le SDK MicrosoftAdvertising fournit divers contrôles que vous pouvez utiliser dans votre application pour afficher les différents types de publicités. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>Étape2: Choisir votre type de publicité et ajouter du code pour afficher des publicités de test dans votre application

Le SDK MicrosoftAdvertising fournit plusieurs types de publicités différents que vous pouvez afficher dans votre application. Choisissez les types de publicités qui conviennent le mieux à votre scénario, puis ajoutez du code à votre application pour afficher ces publicités.

Vous devez spécifier un ID d’application et un ID d’unité publicitaire dans votre code pour servir des publicités à votre application. Lors du développement de votre application, vous devez utiliser les [valeurs de test d’ID d’application et d’ID d’unité publicitaire](test-mode-values.md) pour voir comment votre application restitue les publicités au cours du test.

### <a name="banner-ads"></a>Bannières publicitaires

Ces publicités à affichage statique utilisent une partie de la page d'une application, souvent en haut ou en bas de la page.

Pour des instructions et des exemples de code, voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md) et [AdControl en HTML5 et Javascript](adcontrol-in-html-5-and-javascript.md). Pour obtenir des exemples complets de projet qui montrent comment ajouter des bannières publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir les [Exemples de publicité sur GitHub](http://aka.ms/githubads).

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>Spots

Ce sont des publicités plein écran qui obligent en général l’utilisateur à visionner une vidéo ou à cliquer dessus pour poursuivre sa progression dans l'application ou dans le jeu. Nous prenons en charge deux types de spots publicitaires: bannières et vidéo.

Pour obtenir des instructions et des exemples de code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Publicités natives

Il s’agit de publicités basées sur un composant. Chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel que vous pouvez intégrer dans votre application en utilisant vos propres polices, couleurs et autres composants d’interface utilisateur afin de composer une expérience utilisateur discrète.

Pour obtenir des instructions et des exemples de code, voir [Publicités natives](native-ads.md).

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>Étape3: Obtenir une unité publicitaire à partir du centre de développement et configurer votre application afin de recevoir des publicités dynamiques

Une fois que vous avez testé votre application et que vous êtes prêt à la soumettre au Windows Store, créez une unité publicitaire sur la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) du tableau de bord du Centre de développement Windows. Ensuite, mettez à jour le code de votre application pour utiliser les valeurs ID d’application et ID d'unité publicitaire de cette unité publicitaire. Si vous essayez d’utiliser des valeurs de test d'ID d'application et d'ID d'unité publicitaire dans la version publiée de votre application dans le Windows Store, votre application ne recevra pas de publicités dynamiques. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>Configurer la médiation publicitaire

Par défaut, les bannières publicitaires, les spots et les publicités natives affichent des publicités à partir du réseau de Microsoft pour les publicités payées. Pour optimiser vos revenus publicitaires, vous pouvez activer la médiation publicitaire pour votre unité publicitaire afin d'afficher des publicités à partir de réseaux de publicités payées supplémentaires, tels que Taboola et Smaato. Vous pouvez également augmenter vos capacités de promotion d'application en affichant des publicités provenant des campagnes de promotion d'applications Microsoft.

Pour commencer à utiliser la médiation publicitaire dans votre application UWP, [configurez les paramètres de médiation publicitaire](../publish/monetize-with-ads.md#mediation) de votre unité publicitaire. Par défaut, nous configurons automatiquement les paramètres de médiation à l’aide d’algorithmes d’apprentissage automatique pour vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application. Toutefois, vous avez également la possibilité de choisir manuellement les réseaux à utiliser. Dans chaque cas, les paramètres de médiation sont entièrement configurés dans le service; vous n’avez pas besoin d’apporter des modifications de code dans votre application.    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>La médiation dans les applications Windows8.1 et WindowsPhone8.x

Dans les applications Windows8.1 et WindowsPhone8.x, la médiation publicitaire est uniquement disponible pour les bannières publicitaires. Pour utiliser la médiation publicitaire, vous devez obligatoirement utiliser la classe **AdMediatorControl** dans le [kit SDK MicrosoftAdvertising pour Windows et WindowsPhone8.x](http://aka.ms/store-8-sdk), à la place de la classe **AdControl**. Après avoir ajouté ce contrôle à votre application, vous pouvez configurer manuellement vos paramètres de médiation publicitaire sur le tableau de bord.

Pour plus d’informations concernant l’utilisation de la médiation publicitaire dans une application Windows8.1 ou WindowsPhone8.x, consultez [cet article](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

> [!NOTE]
> La médiation publicitaire pour les applications Windows8.1 et WindowsPhone8.x n’est plus en cours de développement actif. Pour optimiser vos revenus publicitaires potentiels, nous vous recommandons de créer des applications UWP qui utilisent la médiation publicitaire avec des bannières publicitaires, des spots ou des publicités natives.

## <a name="step-4-submit-your-app-and-review-performance"></a>Étape4: Soumettre votre application et examiner les performances

Une fois que vous avez développé votre application contenant des publicités, vous pouvez [soumettre votre application mise à jour](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement pour qu’elle soit disponible dans le Windows Store. Les applications qui affichent des publicités doivent respecter les exigences supplémentaires qui sont spécifiées dans la [section10.10 des politiques du WindowsStore](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) et l’[annexeE du Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx).

Une fois que votre application est publiée et disponible dans le Windows Store, vous pouvez examiner vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord et continuer à apporter des modifications à vos paramètres de médiation afin d'optimiser les performances de vos annonces. Vos revenus publicitaires sont inclus dans votre [résumé du paiement](../publish/payout-summary.md).

<span id="additional-help" />
## <a name="additional-help"></a>Aide supplémentaire

Pour obtenir une aide supplémentaire sur l'utilisation du SDK MicrosoftAdvertising, utilisez les ressources suivantes.

|  Tâche    | Ressource |               
|----------|-------|
| Signaler un bogue ou obtenir un support assisté en matière de publicité     | Visitez la [page de support](https://go.microsoft.com/fwlink/p/?LinkId=331508) et choisissez **Publicité intégrée à l’application**.        |
| Bénéficier du support de la communauté     | Consultez le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     | Voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).       |
| Découvrir les dernières opportunités de monétisation pour les applications Windows     | Consultez [Monétiser vos applications](https://developer.microsoft.com/store/monetize).        |

## <a name="related-topics"></a>Rubriques associées

* [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp)
* [Monétiser votre application grâce aux publicités](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
