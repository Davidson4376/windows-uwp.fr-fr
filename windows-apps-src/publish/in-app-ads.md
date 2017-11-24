---
author: jnHs
Description: "Si votre application affiche des publicités à l’aide du SDKMicrosoftAdvertising, utilisez la page Publicités dans l’application du tableau de bord du Centre de développement pour gérer votre utilisation des publicités."
title: "Publicités dans l’application"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
localizationpriority: high
ms.openlocfilehash: eb835269cd9df7385d8d03e032d8b41cb7f92a19
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2017
---
# <a name="in-app-ads"></a>Publicités dans l’application

Utilisez la page **Monétiser** &gt; **Publicités dans l’application** dans le tableau de bord du Centre de développement pour créer et gérer des unités publicitaires pour:

* Les applications de plateforme Windows universelle (UWP) qui utilisent le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* Les applications Windows8.x et WindowsPhone8.x qui utilisent le [SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk).

Pour plus d’informations sur la façon d’intégrer ces SDK avec vos applications pour afficher des publicités, voir [Afficher des publicités dans votre application avec le SDK Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>Créer des unités publicitaires

Pour créer une unité publicitaire pour une [bannière publicitaire](../monetize/banner-ads.md), un [spot](../monetize/interstitial-ads.md) ou une [publicité native](../monetize/native-ads.md) dans votre application:

1.  Accédez à la page **Monétiser** &gt; **Publicités dans l’application** du tableau de bord et cliquez sur **Créer une publicité**.

2.  Dans la liste déroulante **Nom de l'application**, sélectionnez l’application dans laquelle votre unité publicitaire sera utilisée.

3.  Dans le champ **Nom de la publicité**, entrez un nom pour l’unité publicitaire. Il peut s’agir d’une chaîne descriptive que vous souhaitez utiliser pour identifier l’unité publicitaire à des fins de création de rapports.

4.  Dans la liste déroulante **Type de publicité**, sélectionnez le type de publicité.

    * Si vous utilisez un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) dans votre application pour afficher des bannières publicitaires, sélectionnez **Bannière**.

    * Si vous utilisez un [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) dans votre application pour afficher des spots vidéo ou des bannières spots publicitaires, sélectionnez **Spot vidéo** ou **Spot sous forme de bannière** (veillez à sélectionner l’option appropriée pour le type de spot à afficher).

    * Si vous utilisez un [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) dans votre application pour afficher des publicités natives, sélectionnez **Native**.
      > [!NOTE]
      > La possibilité de créer des unités publicitaires **natives** n'est actuellement disponible que pour des développeurs sélectionnés qui participent à un programme pilote, mais nous envisageons de la rendre disponible pour tous les développeurs bientôt. Si vous souhaitez rejoindre notre programme pilote, contactez-nous à l’adresse aiacare@microsoft.com.

5. Dans la liste déroulante **Famille d’appareils**, sélectionnez la famille d’appareils ciblée par l’application dans laquelle votre unité publicitaire sera utilisée. Les options disponibles sont: **UWP (Windows10)**, **PC/tablette (Windows8.1)** ou **Mobile (Windows Phone8.x)**.

6. Configurez les paramètres supplémentaires suivants selon vos besoins:

    * Si vous sélectionnez la famille d’appareils **UWP (Windows10)** pour l’unité publicitaire, vous pouvez éventuellement configurer les [paramètres de médiation](#mediation) pour l’unité publicitaire.

    * Si vous sélectionnez la famille d’appareils **PC/tablette (Windows8.1)** ou **Mobile (Windows Phone8.x)** pour une unité publicitaire bannière, vous pouvez éventuellement sélectionner **Afficher des annonces de la communauté dans votre application** pour choisir les [publicités de la communauté](about-community-ads.md).

7.  Si vous n’avez pas encore défini la conformité avec la réglementation COPPA pour l’application sélectionnée, choisissez une option dans la section [Conformité avec la réglementation COPPA](#coppa).

8.  Cliquez sur **Créer une publicité**.

Après avoir créé la nouvelle unité publicitaire, celle-ci apparaît dans le tableau des unités publicitaires disponibles dans la page **Monétiser** &gt; **Publicités dans l’application**.

<span id="available-ad-units" />
## <a name="review-and-edit-ad-units"></a>Examiner et éditer des unités publicitaires

Après avoir créé des unités publicitaires pour une ou plusieurs applications dans votre compte, ces unités publicitaires apparaissent dans un tableau au bas de la page **Monétiser** &gt; **Publicités dans l’application**. Ce tableau affiche l’**ID de l’application** et l’**ID de la publicité** pour chaque unité publicitaire, ainsi que d’autres informations. Pour afficher les publicités dans votre application, vous devrez utiliser ces valeurs dans votre code. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](../monetize/set-up-ad-units-in-your-app.md).

* Si votre application affiche des [bannières publicitaires](../monetize/banner-ads.md), affectez ces valeurs aux propriétés [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) et [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de votre objet [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).

* Si votre application affiche des [spots](../monetize/interstitial-ads.md), transmettez ces valeurs à la méthode [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de votre objet [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

* Si votre application affiche des [publicités natives](../monetize/native-ads.md), transmettez ces valeurs aux paramètres *applicationId* et *adUnitId* du constructeur [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx).
  > [!IMPORTANT]
  > Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas diffusées pour cette unité publicitaire.

  > [!NOTE]
  > Vous pouvez utiliser plusieurs contrôles de bannière publicitaire, de spot et de publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous proposons à votre application.

Pour modifier les [paramètres de médiation](#mediation) pour une unité publicitaire UWP ou la [conformité à la réglementation COPPA](#coppa) pour l’application dans laquelle l’unité publicitaire est utilisée, cliquez sur le nom de l'unité publicitaire.

<span id="mediation" />
## <a name="mediation-settings"></a>Paramètres de médiation

Lorsque vous [créez une unité publicitaire UWP](#create-ad-unit) ou [modifiez une unité publicitaire UWP existante](#available-ad-units), utilisez les options de cette section pour configurer la médiation publicitaire pour l’unité publicitaire. La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des publicités issues de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux publicitaires payés et les publicités sans génération de revenus des campagnes de promotion d’applications Microsoft. Nous nous chargeons de la médiation des demandes de bannières publicitaires des réseaux publicitaires que vous choisissez. Si vous disposez d’une unité publicitaire UWP déjà associée à une bannière publicitaire, à un spot ou à une publicité native dans votre application, l’activation de la médiation publicitaire ne nécessite aucune modification de code dans votre application.

> [!NOTE]
> Lorsque vous activez la médiation publicitaire pour une unité publicitaire UWP, vous n’avez pas besoin d’obtenir une unité publicitaire auprès de réseaux publicitaires tiers. Notre service de médiation publicitaire crée automatiquement les unités publicitaires tierces nécessaires.

Pour configurer les paramètres de médiation publicitaire pour une unité publicitaire UWP dans votre application:

1. [Créez une unité publicitaire](#create-ad-unit) ou [sélectionnez une unité publicitaire existante](#available-ad-units).

2. Dans la page **Publicités dans l’application**, accédez à la section **Paramètres de médiation**.

3. Par défaut, la case à cocher **Laisser Microsoft choisir les meilleurs paramètres de médiation pour votre application** est activée. Cette option utilise des algorithmes d’apprentissage de l’ordinateur pour choisir automatiquement les paramètres de médiation publicitaire pour votre application, afin de vous aider à optimiser vos revenus publicitaires sur les marchés que votre application prend en charge. Nous vous recommandons d’utiliser cette option. Dans le cas contraire, si vous souhaitez choisir vos propres paramètres de médiation publicitaire, désactivez cette case à cocher.
    > [!NOTE]
    > Les étapes restantes de cette section s'appliquent uniquement si vous désactivez cette case à cocher et que vous choisissez votre propres paramètres de médiation publicitaire.

4. Dans la liste déroulante **Cible**, choisissez **Ligne de base** pour définir la configuration par défaut de vos paramètres de médiation publicitaire. Cette configuration par défaut s’appliquera à tous les marchés, à l’exception des marchés pour lesquels vous définissez des configurations spécifiques au marché.

6. Ensuite, spécifiez le taux de publicités que vous voulez afficher dans votre contrôle à partir de réseaux payants (qui vous rémunèrent pour les expositions) et d’autres réseaux publicitaires (qui ne vous rémunèrent pas pour les expositions). Pour ce faire, entrez une valeur comprise entre 0 et 100dans le champ **Poids** pour **Réseaux publicitaires payants** et **Autres réseaux publicitaires**.  

7. Dans la section **Réseaux publicitaires payants**, cochez la case dans la colonne **Actif** pour chaque [réseau payant](#paid-networks) que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle).

8. Si vous avez sélectionné une unité publicitaire **Bannière** ou **Spot sous forme de bannière**, vous voyez également une section intitulée **Autres réseaux publicitaires**. Les réseaux de cette section ne vous font gagner aucun revenu pour les expositions publicitaires. Ces réseaux affichent plutôt des publicités provenant de sources telles que des campagnes de promotion d’applications.

    Dans la section **Autres réseaux publicitaires**, cochez la case dans la colonne **Actif** pour chaque [autre réseau](#other-networks) que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle). Les autres réseaux actuellement pris en charge sont les suivants:

9. Pour chaque marché dans lequel vous souhaitez remplacer la configuration de médiation par défaut, sélectionnez le marché dans la liste déroulante **Cible** et mettez à jour les sélections de réseaux publicitaires et le classement.

10. Cliquez sur **Créer une publicité** (si vous créez une unité publicitaire) ou **Enregistrer** (si vous modifiez une unité publicitaire existante).

<span id="paid-networks" />
### <a name="supported-paid-ad-networks"></a>Réseaux publicitaires payants pris en charge

Le tableau suivant répertorie les réseaux payants actuellement pris en charge pour chaque type de publicité. Notez que certains de ces réseaux ne sont [pas disponibles dans tous les marchés](#network-markets).

|  Réseau publicitaire  |  Description  |  Types de publicités pris en charge  |
|--------------|---------------|---------------------|
| AOL et AppNexus |  Il s’agit d’un réseau publicitaire géré par Microsoft qui diffuse des annonces par le biais de nos réseaux partenaires, AOL et AppNexus.<p/>**Remarque**: AOL et AppNexus figure toujours au premier rang de la liste **Réseaux publicitaires payants** pour les unités publicitaires Bannière et il ne peut pas être classé à un niveau inférieur pour ces types de publicités. | Bannière, spot vidéo |
| AppNexus (direct) | Sélectionnez cette option pour proposer des spots vidéo publicitaires à partir de [AppNexus](https://www.appnexus.com). | Spot vidéo, native  |
| Publicités pour l’installation d’applications Microsoft | Sélectionnez cette option pour proposer des publicités pour l’installation d’applications ou des publicités pour la réactivation d’applications créées par d’autres développeurs dans l’écosystème Windows qui [créent des campagnes de publicité pour leurs applications](create-an-ad-campaign-for-your-app.md).  |  Bannière, spot bannière, native  |
| Smaato |  Sélectionnez cette option pour proposer des publicités à partir de [Smaato](https://www.smaato.com/). |  Bannière  |
| smartclip |  Sélectionnez cette option pour proposer des publicités à partir de [smartclip](http://www.smartclip.com/). |  Spot vidéo  |
| SpotX |  Sélectionnez cette option pour proposer des publicités à partir de [SpotX](https://www.spotx.tv/). |  Spot vidéo  |
| Taboola |  Sélectionnez cette option pour proposer des publicités à partir de [Taboola](https://www.taboola.com/). |  Bannière  |

<span id="other-networks" />
### <a name="other-ad-networks"></a>Autres réseaux publicitaires

Le tableau suivant répertorie les autres réseaux actuellement pris en charge pour chaque type de publicité.

|  Réseau publicitaire  |  Description  |  Types de publicités pris en charge  |
|--------------|---------------|---------------------|
| Publicités de la communautéMicrosoft |  Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire de la communauté](about-community-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne. | Bannière, spot bannière |
| Publicités maison de Microsoft | Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire maison](about-house-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne. | Bannière, spot bannière  |


<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>Marchés pris en charge pour les réseaux publicitaires

Les réseaux publicitaires disponibles proposent des publicités dans tous les [marchés pris en charge](define-pricing-and-market-selection.md#microsoft-store-consumer-markets), avec les exceptions suivantes.

|  Réseau publicitaire  |  Marchés pris en charge  |
|--------------|---------------------|
| Smaato | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis |
| smartclip | Autriche, Belgique, Danemark, Finlande, Allemagne, Italie, Pays-Bas, Norvège, Suède, Suisse  |

<span id="coppa" />
## <a name="coppa-compliance"></a>Conformité avec la réglementation COPPA

Dans le cadre de la réglementation COPPA (Online Privacy Protection Act) relative à la protection de la vie privée des enfants sur Internet, vous devez avertir Microsoft si votre application s’adresse aux enfants de moins de 13 ans. Si vous utilisez le Centre de développement pour indiquer à Microsoft que votre application s’adresse aux enfants de moins de 13 ans, Microsoft prendra les mesures nécessaires pour désactiver les services de publicité comportementale lors de la diffusion de publicités dans votre application. Si votre application s’adresse aux enfants de moins de 13 ans, la réglementation COPPA vous impose certaines obligations.

Pour plus d’informations sur les obligations qui vous incombent en vertu de la réglementation COPPA, veuillez consulter [cette page](http://go.microsoft.com/fwlink/p/?linkid=536558).
