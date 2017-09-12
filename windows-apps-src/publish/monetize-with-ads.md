---
author: jnHs
Description: "Si votre application utilise la médiation publicitaire ou affiche des bannières publicitaires ou des spots à l’aide de Microsoft Store Services SDK, utilisez la page Monétiser avec des publicités afin de gérer votre utilisation des publicités."
title: "Monétiser à l’aide des publicités"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>Monétiser à l’aide des publicités

Chaque application de votre tableau de bord inclut une page **Monétisation** &gt; **Monétiser avec des publicités**. Utilisez cette page afin de gérer votre utilisation des publicités pour les scénarios de développement ci-après dans votre application:

* Votre application UWP utilise une classe [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) ou [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) du [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp).
* Votre application Windows8.x ou WindowsPhone8.x utilise une classe **AdControl** ou **InterstitialAd** du [SDK MicrosoftAdvertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk).
* Votre application Windows8.x ou Windows Phone8.x utilise une classe **AdMediatorControl** du [SDK MicrosoftAdvertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk).

Pour plus d’informations sur ces scénarios de développement, consultez l’article [Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>Créer des unités publicitaires

Utilisez cette section afin de créer une unité publicitaire pour les scénarios suivants:

* Votre application affiche des bannières publicitaires à l’aide d’une classe **AdControl**. Pour plus d’informations, voir [AdControl en XAML et .NET](../monetize/adcontrol-in-xaml-and--net.md) et [AdControl en HTML5 et JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
* Votre application affiche des spots vidéo ou des bannières publicitaires à l’aide d’une classe **InterstitialAd**. Pour plus d’informations, consultez l’article [Spots](../monetize/interstitial-ads.md).
* Votre application affiche des publicités natives à l’aide d’une classe **NativeAd**. Pour plus d’informations, consultez l’article [Publicités natives](../monetize/native-ads.md).

Pour créer une unité publicitaire:

1.  Dans le champ **Nom de la publicité**, entrez un nom pour l’unité publicitaire. Il peut s’agir d’une chaîne descriptive que vous souhaitez utiliser pour identifier l’unité publicitaire à des fins de création de rapports.
2.  Dans la liste déroulante **Type de publicité**, sélectionnez le type d’unité publicitaire correspondant aux publicités que vous affichez dans votre contrôle. Les options disponibles sont les suivantes: **Bannière**, **Spot sous forme de bannière**, **Spot vidéo** et **Natif**.
    > [!NOTE]
    > Pour l’instant, la possibilité de créer des unités publicitaires **natives** n’est disponible que pour des développeurs sélectionnés qui participent à un programme pilote, mais nous envisageons de la rendre accessible à tous les développeurs prochainement. Si vous souhaitez rejoindre notre programme pilote, contactez-nous à l’adresse aiacare@microsoft.com.

3.  Dans la liste déroulante **Famille d’appareils**, sélectionnez la famille d’appareils ciblée par l’application dans laquelle votre unité publicitaire sera utilisée. Les options disponibles sont: **UWP (Windows10)**, **PC/tablette (Windows8.1)** ou **Mobile (Windows Phone8.x)**.
4.  Cliquez sur **Créer une publicité**.

La nouvelle unité publicitaire apparaît en haut de la liste dans la section **Publicités disponibles** de cette page. Pour plus d’informations sur l’utilisation des unités publicitaires dans votre application, consultez l’article [Configurer des unités publicitaires dans votre application](../monetize/set-up-ad-units-in-your-app.md).

> [!NOTE]
> Si votre application Windows8.x ou Windows Phone8.x utilise une classe **AdMediatorControl** pour afficher des bannières publicitaires, vous n’avez pas besoin de demander des unités publicitaires ici. Dans ce scénario, les unités publicitaires sont automatiquement générées pour vous.

<span id="available-ad-units" />
## <a name="available-ad-units"></a>Unités publicitaires disponibles

Vos unités publicitaires apparaissent dans un tableau situé en bas de cette section. Pour chaque unité publicitaire, vous voyez l’**ID de l’application** et l’**ID de l’unité publicitaire**. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans votre code :

-   Si votre application affiche des bannières, affectez ces valeurs aux propriétés [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) et [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de votre objet [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Pour plus d’informations, voir [AdControl en XAML et .NET](../monetize/adcontrol-in-xaml-and--net.md) et [AdControl en HTML5 et JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
-   Si votre application affiche des spots publicitaires, transmettez ces valeurs à la méthode [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de votre objet [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Pour plus d’informations, consultez l’article [Spots](../monetize/interstitial-ads.md).
-   Si votre application affiche des publicités natives, transmettez ces valeurs aux paramètres *applicationId* et *adUnitId* du constructeur [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Pour plus d’informations, consultez l’article [Publicités natives](../monetize/native-ads.md).

> [!IMPORTANT]
> Vous ne pouvez utiliser chaque unité publicitaire que dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas diffusées pour cette unité publicitaire.

> [!NOTE]
> Vous pouvez utiliser plusieurs contrôles de bannière publicitaire, de spot et de publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/monetize-with-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous diffusons à votre application.

<span id="mediation" />
## <a name="ad-mediation"></a>Médiation publicitaire

Si votre application est une application UWP pour Windows10, vous pouvez utiliser les options de cette section afin d’activer la médiation publicitaire pour une unité publicitaire UWP associée à une bannière publicitaire, à un spot ou à une publicité native dans votre application. La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des publicités issues de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux publicitaires payés et les publicités sans génération de revenus des campagnes de promotion d’applications Microsoft. Nous nous chargeons de la médiation des demandes de bannières publicitaires des réseaux publicitaires que vous choisissez.

Si vous disposez d’une unité publicitaire UWP déjà associée à une bannière publicitaire, à un spot ou à une publicité native dans votre application, l’activation de la médiation publicitaire ne nécessite aucune modification de code dans votre application.

> [!NOTE]
> Cette section décrit les options de **Médiation publicitaire** pour les packages d’application UWP. Si votre package d’application cible Windows8.x ou Windows Phone8.x et utilise **AdMediatorControl** du [SDK MicrosoftAdvertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk), la section **Médiation publicitaire** dans le tableau de bord affiche un ensemble d’options différent. Pour plus d’informations sur la configuration des paramètres de médiation pour un package d’application Windows8.x ou Windows Phone8.x qui utilise **AdMediatorControl**, consultez [cet article](https://msdn.microsoft.com/library/windows/apps/mt219689).

Pour configurer les paramètres de médiation publicitaire pour une unité publicitaire UWP dans votre application:

1. Dans la liste déroulante **Configurer la médiation pour**, sélectionnez le package d’application UWP qui contient l’unité de bannière publicitaire, de spot ou d’unité publicitaire native que vous souhaitez configurer.

2. Dans la liste déroulante **Type de publicité**, sélectionnez le type d’unité publicitaire que vous souhaitez configurer (**Bannière**, **Spot sous forme de bannière**, **Spot vidéo** ou **Natif**).

3. Dans la liste déroulante **Unité publicitaire**, sélectionnez le nom de l’unité publicitaire UWP que vous souhaitez configurer.
    > [!NOTE]
    > Lorsque vous activez la médiation publicitaire pour une unité publicitaire UWP, vous n’avez pas besoin d’obtenir une unité publicitaire auprès de réseaux publicitaires tiers. Notre service de médiation publicitaire crée automatiquement les unités publicitaires tierces nécessaires.

4. Par défaut, la case à cocher **Laisser Microsoft choisir les meilleurs paramètres de médiation pour votre application** est activée. Cette option utilise des algorithmes d’apprentissage de l’ordinateur pour choisir automatiquement les paramètres de médiation publicitaire pour votre application, afin de vous aider à optimiser vos revenus publicitaires sur les marchés que votre application prend en charge. Nous vous recommandons d’utiliser cette option. Dans le cas contraire, si vous souhaitez choisir vos propres paramètres de médiation publicitaire, désactivez cette case à cocher.
    > [!NOTE]
    > Les étapes restantes de cette section s'appliquent uniquement si vous désactivez cette case à cocher et que vous choisissez votre propres paramètres de médiation publicitaire.

5. Dans la liste déroulante **Cible**, choisissez **Ligne de base** pour définir la configuration par défaut de vos paramètres de médiation publicitaire. Cette configuration par défaut s’appliquera à tous les marchés, à l’exception des marchés pour lesquels vous définissez des configurations spécifiques au marché.

6. Ensuite, spécifiez le taux de publicités que vous voulez afficher dans votre contrôle à partir de réseaux payants (qui vous rémunèrent pour les expositions) et d’autres réseaux publicitaires (qui ne vous rémunèrent pas pour les expositions). Pour ce faire, entrez une valeur comprise entre 0 et 100dans le champ **Poids** pour **Réseaux publicitaires payants** et **Autres réseaux publicitaires**.  

7. Dans la section **Réseaux publicitaires payants**, sélectionnez la case à cocher dans la colonne **Actif** pour chaque réseau payant que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle).

    Les réseaux payants suivants sont actuellement pris en charge. Notez que certains de ces réseaux ne sont [pas disponibles dans tous les marchés](#network-markets).

    -   **AOL et AppNexus**. Il s’agit d’un réseau publicitaire géré par Microsoft qui diffuse des annonces par le biais de nos réseaux partenaires, AOL et AppNexus. Ce réseau est pris en charge pour les unités publicitaires **Bannière** et **Spot vidéo**.
        > [!NOTE]
        > **AOL et AppNexus** figure toujours au premier rang de la liste **Réseaux publicitaires payants** pour les unités publicitaires **Bannière** et il ne peut pas être classé à un niveau inférieur pour ces types de publicités.

    -   **AppNexus (direct)**: sélectionnez cette option pour proposer des spots vidéo publicitaires à partir [d’AppNexus](https://www.appnexus.com). Ce réseau est pris en charge pour les unités publicitaires **Spot vidéo** et **Natif**.

    -   **Publicités pour l’installation de Microsoft Apps**. Sélectionnez cette option pour proposer des publicités pour l’installation d’applications ou des publicités pour la réactivation d’applications créées par d’autres développeurs dans l’écosystème Windows qui [créent des campagnes de publicité pour leurs applications](create-an-ad-campaign-for-your-app.md). Ce réseau est pris en charge pour les unités publicitaires **Bannière**, **Spot vidéo** et **Natif**.

    -   **Smaato**: sélectionnez cette option pour proposer des publicités à partir de [Smaato](https://www.smaato.com/). Ce réseau est pris en charge pour les unités publicitaires **Bannière**.

    -   **smartclip**: sélectionnez cette option pour proposer des publicités à partir de [smartclip](http://www.smartclip.com/). Ce réseau est pris en charge pour les unités publicitaires **Spot vidéo**.

    -   **SpotX**: sélectionnez cette option pour proposer des publicités à partir de [SpotX](https://www.spotx.tv/). Ce réseau est pris en charge pour les unités publicitaires **Spot vidéo**.

    -   **Taboola**: sélectionnez cette option pour proposer des publicités à partir de [Taboola](https://www.taboola.com/). Ce réseau est pris en charge pour les unités publicitaires **Bannière**.

8. Si vous avez sélectionné une unité publicitaire **Bannière** ou **Spot sous forme de bannière**, vous voyez également une section intitulée **Autres réseaux publicitaires**. Les réseaux de cette section ne vous font gagner aucun revenu pour les expositions publicitaires. Ces réseaux affichent plutôt des publicités provenant de sources telles que des campagnes de promotion d’applications. 

    Dans la section **Autres réseaux publicitaires**, cochez la case dans la colonne **Actif** pour chaque autre réseau que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle). Les autres réseaux actuellement pris en charge sont les suivants:

    -   **Publicités Microsoft Community**. Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire de la communauté](about-community-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne. 

    -   **Publicités maison de Microsoft**. Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire maison](about-house-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne.

9. Pour chaque marché dans lequel vous souhaitez remplacer la configuration de médiation par défaut, sélectionnez le marché dans la liste déroulante **Cible** et mettez à jour les sélections de réseaux publicitaires et le classement.

10. Cliquez sur **Enregistrer**.

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>Marchés pris en charge pour les réseaux publicitaires

Les réseaux publicitaires disponibles proposent des publicités dans tous les [marchés pris en charge](define-pricing-and-market-selection.md#windows-store-consumer-markets) pour les applications UWP, avec les exceptions suivantes.

|  Réseau publicitaire  |  Marchés pris en charge  |
|--------------|---------------------|
| Smaato | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis |
| smartclip | Autriche, Belgique, Danemark, Finlande, Allemagne, Italie, Pays-Bas, Norvège, Suède, Suisse  |


## <a name="microsoft-affiliate-ads"></a>Annonces des affiliés Microsoft

Cochez la case dans cette section si vous voulez afficher les annonces des affiliés Microsoft dans votre application. Si vous activez cette case à cocher, les annonces concernant les produits disponibles dans le Store, y compris la musique, les jeux, les films, les applications, le matériel et les logiciels, sont affichées dans votre application si aucune annonce n’est disponible via les autres réseaux publicitaires. Lorsque des clients cliquent sur les publicités et achètent des produits au sein d’une fenêtre d’attribution donnée du WindowsStore, vous touchez une commission sur les achats approuvés.

Si vous changez cette sélection, il n’est pas nécessaire de republier l’application pour que les modifications prennent effet. Pour plus d’informations sur les annonces des affiliés Microsoft, voir [À propos des annonces des affiliés](about-affiliate-ads.md).

## <a name="coppa-compliance"></a>Conformité avec la réglementation COPPA

Dans le cadre de la réglementation COPPA (Online Privacy Protection Act) relative à la protection de la vie privée des enfants sur Internet, vous devez avertir Microsoft si votre application s’adresse aux enfants de moins de 13 ans. Si vous utilisez le Centre de développement pour indiquer à Microsoft que votre application s’adresse aux enfants de moins de 13 ans, Microsoft prendra les mesures nécessaires pour désactiver les services de publicité comportementale lors de la diffusion de publicités dans votre application. Si votre application s’adresse aux enfants de moins de 13 ans, la réglementation COPPA vous impose certaines obligations.

Pour plus d’informations sur les obligations qui vous incombent en vertu de la réglementation COPPA, veuillez consulter [cette page](http://go.microsoft.com/fwlink/p/?linkid=536558).
