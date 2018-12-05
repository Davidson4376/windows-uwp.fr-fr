---
Description: If your app displays ads using the Microsoft Advertising SDK, use the In-app ads page of Partner Center to manage your use of ads.
title: Publicités dans l’application
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78d825adf023b3c27ba6535f4cfdcc3a99be2d6c
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710549"
---
# <a name="in-app-ads"></a>Publicités dans l’application

Utilisez la **MONÉTISER** &gt; page **publicités dans l’application** dans [L’espace partenaires](https://partner.microsoft.com/dashboard) pour créer et gérer des unités publicitaires pour:

* Les applications de plateforme Windows universelle (UWP) qui utilisent le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* Publié sur Windows 8.x et des applications Windows Phone 8.x qui utilisent la [SDK Microsoft Advertising pour Windows et Windows Phone 8.x](http://aka.ms/store-8-sdk).

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créés ne peuvent pas inclure des packages ciblant 8.x/Windows Windows Phone 8.x ou version antérieure. Pour plus d’informations, consultez le [billet de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Pour plus d’informations sur la façon d’intégrer ces SDK avec vos applications pour afficher des publicités, voir [Afficher des publicités dans votre application avec le SDK Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Créer des unités publicitaires

Pour créer une unité publicitaire pour une [bannière publicitaire](../monetize/banner-ads.md), un [spot](../monetize/interstitial-ads.md) ou une [publicité native](../monetize/native-ads.md) dans votre application:

1.  Accédez à la **MONÉTISER** &gt; **publicités In-app** de page dans l’espace partenaires et cliquez sur **créer une publicité**.
2.  Dans la liste déroulante **Nom de l'application**, sélectionnez l’application dans laquelle votre unité publicitaire sera utilisée.
3.  Dans le champ **Nom de la publicité**, entrez un nom pour l’unité publicitaire. Il peut s’agir d’une chaîne descriptive que vous souhaitez utiliser pour identifier l’unité publicitaire à des fins de création de rapports.
4.  Dans la liste déroulante **Type de publicité**, sélectionnez le type de publicité.

    * Si vous affichez une bannière publicitaire dans votre application, sélectionnez la **bannière**.
    * Si vous affichez un spot vidéo ou une bannière publicitaire dans votre application, sélectionnez **spot vidéo** ou une **bannière** (veillez à sélectionner l’option appropriée pour le type de spot publicitaire que vous souhaitez afficher).
    * Si vous affichez une publicité native dans votre application, sélectionnez **natif**.

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

* Si votre application affiche des [bannières publicitaires](../monetize/banner-ads.md), affectez ces valeurs aux propriétés [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) et [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) de votre objet [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol).
* Si votre application affiche des [spots](../monetize/interstitial-ads.md), transmettez ces valeurs à la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) de votre objet [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad).
* Si votre application affiche [des publicités natives](../monetize/native-ads.md), transmettez ces valeurs au constructeur **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas diffusées pour cette unité publicitaire.

  > [!NOTE]
  > Vous pouvez utiliser plusieurs contrôles de bannière publicitaire, de spot et de publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous proposons à votre application.

Pour modifier les [paramètres de médiation](#mediation) pour une unité publicitaire UWP ou la [conformité à la réglementation COPPA](#coppa) pour l’application dans laquelle l’unité publicitaire est utilisée, cliquez sur le nom de l'unité publicitaire.

> [!NOTE]
> Si une unité publicitaire n’a aucune activité pour les six derniers mois, nous des étiquettes comme **inactif**et supprimerons de l’espace partenaires. Vous pouvez utiliser des filtres pour afficher uniquement les unités publicitaires **Actives** ou **Inactives**. Si vous voyez des unités publicitaires qui, à votre avis, sont marquées à tort comme **Inactives**, [contactez le support](http://aka.ms/storesupport).

<span id="mediation" />

## <a name="mediation-settings"></a>Paramètres de médiation

Lorsque vous [créez une unité publicitaire UWP](#create-ad-unit) ou [Modifier une unité de publicitaire UWP existante](#available-ad-units), utilisez les options de cette section pour configurer la [médiation publicitaire](../monetize/ad-mediation-service.md) pour l’unité publicitaire. La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des publicités issues de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux publicitaires payés et les publicités sans génération de revenus des campagnes de promotion d’applications Microsoft. Nous nous chargeons de la médiation des demandes de bannières publicitaires des réseaux publicitaires que vous choisissez. Si vous disposez d’une unité publicitaire UWP déjà associée à une bannière publicitaire, à un spot ou à une publicité native dans votre application, l’activation de la médiation publicitaire ne nécessite aucune modification de code dans votre application.

> [!NOTE]
> Lorsque vous activez la médiation publicitaire pour une unité publicitaire UWP, vous n’avez pas besoin d’obtenir une unité publicitaire auprès de réseaux publicitaires tiers. Notre service de médiation publicitaire crée automatiquement les unités publicitaires tierces nécessaires.

Pour configurer les paramètres de médiation publicitaire pour une unité publicitaire UWP dans votre application:

1. [Créez une unité publicitaire](#create-ad-unit) ou [sélectionnez une unité publicitaire existante](#available-ad-units).
2. Sur la page **publicités In-app** , accédez à la section **paramètres de médiation** et la configuration de vos paramètres.

    * Par défaut, la case à cocher **laisser Microsoft optimiser mes paramètres** est sélectionnée. Nous vous recommandons d’utiliser cette option. Cette option utilise des algorithmes d’apprentissage de l’ordinateur pour choisir automatiquement les paramètres de médiation publicitaire pour votre application, afin de vous aider à optimiser vos revenus publicitaires sur les marchés que votre application prend en charge. Lorsque vous utilisez cette option, vous pouvez également choisir les réseaux publicitaires que vous voulez utiliser dans la configuration. Désactivez les réseaux publicitaires que vous ne voulez pas faire partie de la configuration et notre algorithme permet de garantir que votre application reçoit uniquement des publicités à partir de réseaux publicitaires sélectionné.
    * Si vous souhaitez choisir vos propres ad paramètres de médiation, choisir de **Modifier les paramètres par défaut**.

    > [!NOTE]
    > Les étapes restantes de cette section sont appliquent si vous décidez de **Modifier les paramètres par défaut**uniquement.

3. Dans la liste déroulante **Cible**, choisissez **Ligne de base** pour définir la configuration par défaut de vos paramètres de médiation publicitaire. Cette configuration par défaut s’appliquera à tous les marchés, à l’exception des marchés pour lesquels vous définissez des configurations spécifiques au marché.
4. Ensuite, spécifiez le taux de publicités que vous voulez afficher dans votre contrôle à partir de réseaux payants (qui vous rémunèrent pour les expositions) et d’autres réseaux publicitaires (qui ne vous rémunèrent pas pour les expositions). Pour ce faire, entrez une valeur comprise entre 0 et 100dans le champ **Poids** pour **Réseaux publicitaires payants** et **Autres réseaux publicitaires**.  
5. Dans la section **Réseaux publicitaires payants**, cochez la case dans la colonne **Actif** pour chaque [réseau payant](#paid-networks) que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle).
6. Si vous avez sélectionné une unité publicitaire **Bannière** ou **Spot sous forme de bannière**, vous voyez également une section intitulée **Autres réseaux publicitaires**. Les réseaux de cette section ne vous font gagner aucun revenu pour les expositions publicitaires. Ces réseaux affichent plutôt des publicités provenant de sources telles que des campagnes de promotion d’applications.

    Dans la section **Autres réseaux publicitaires**, cochez la case dans la colonne **Actif** pour chaque [autre réseau](#other-networks) que vous souhaitez utiliser, puis utilisez les flèches dans la colonne **Rang** pour trier les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle). Les autres réseaux actuellement pris en charge sont les suivants:

7. Pour chaque marché dans lequel vous souhaitez remplacer la configuration de médiation par défaut, sélectionnez le marché dans la liste déroulante **Cible** et mettez à jour les sélections de réseaux publicitaires et le classement.
8. Cliquez sur **Créer une publicité** (si vous créez une unité publicitaire) ou **Enregistrer** (si vous modifiez une unité publicitaire existante).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Réseaux publicitaires payants pris en charge

Le tableau suivant répertorie les réseaux payants actuellement pris en charge pour chaque type de publicité. Notez que certains de ces réseaux ne sont [pas disponibles dans tous les marchés](#network-markets).

|  Réseau publicitaire  |  Description  |  Types de publicités pris en charge  |
|--------------|---------------|---------------------|
| Formule et AppNexus |  Il s’agit d’un réseau publicitaire géré par Microsoft qui diffuse des annonces par le biais de notre partenaire réseaux, formule et AppNexus.<p/>**Remarque**: formule et AppNexus est toujours au premier rang de la liste des **réseaux publicitaires de payé** pour les unités de bannières publicitaires, et il ne peut pas être modifié à un niveau inférieur pour ces types de publicités. | Bannière, spot vidéo |
| AppNexus (direct) | Sélectionnez cette option pour proposer des publicités à partir de [AppNexus](https://www.appnexus.com). | Spot vidéo, native  |
| Publicités pour l’installation d’applications Microsoft | Sélectionnez cette option pour proposer des publicités pour l’installation d’applications ou des publicités pour la réactivation d’applications créées par d’autres développeurs dans l’écosystème Windows qui [créent des campagnes de publicité pour leurs applications](create-an-ad-campaign-for-your-app.md).  |  Bannière, spot bannière, native  |
| Recommandations de contenu MSN |  Sélectionnez cette option pour proposer des publicités à partir des recommandations de contenu MSN. |  Bannière, spot bannière  |
| Outbrain |  Sélectionnez cette option pour proposer des publicités à partir de [Outbrain](https://www.outbrain.com/). |  Bannière, spot bannière  |
| Revcontent |  Sélectionnez cette option pour proposer des publicités à partir de [Revcontent](http://www.revcontent.com/). |  Bannière, native  |
| Smaato |  Sélectionnez cette option pour proposer des publicités à partir de [Smaato](https://www.smaato.com/). |  Bannière  |
| smartclip |  Sélectionnez cette option pour proposer des publicités à partir de [smartclip](http://www.smartclip.com/). |  Spot vidéo  |
| SpotX |  Sélectionnez cette option pour proposer des publicités à partir de [SpotX](https://www.spotx.tv/). |  Spot vidéo  |
| Taboola |  Sélectionnez cette option pour proposer des publicités à partir de [Taboola](https://www.taboola.com/). |  Bannière  |
| Undertone | Sélectionnez cette option pour proposer des publicités à partir de [Undertone](https://www.undertone.com/). | Bannière |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Autres réseaux publicitaires

Le tableau suivant répertorie les autres réseaux actuellement pris en charge pour chaque type de publicité.

|  Réseau publicitaire  |  Description  |  Types de publicités pris en charge  |
|--------------|---------------|---------------------|
| Publicités de la communautéMicrosoft |  Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire de la communauté](about-community-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne. | Bannière, spot bannière |
| Publicités maison de Microsoft | Si vous [créez une campagne de publicité pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire maison](about-house-ads.md), sélectionnez ces options pour afficher des publicités à partir de cette campagne. | Bannière, spot bannière  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Marchés pris en charge pour les réseaux publicitaires

Les réseaux publicitaires disponibles proposent des publicités dans tous les [marchés pris en charge](define-market-selection.md#microsoft-store-consumer-markets), avec les exceptions suivantes.

|  Réseau publicitaire  |  Marchés pris en charge  |
|--------------|---------------------|
| Revcontent | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis  |
| Smaato | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis |
| smartclip | Autriche, Belgique, Danemark, Finlande, Allemagne, Italie, Pays-Bas, Norvège, Suède, Suisse  |
| Undertone | États-Unis |

<span id="coppa" />

## <a name="coppa-compliance"></a>Conformité avec la réglementation COPPA

Lorsque vous [créez une unité publicitaire](#create-ad-unit) , ou [Sélectionnez une unité publicitaire existante](#available-ad-units), la section de la **conformité de la réglementation COPPA** s’affiche en bas de la page si l’application sélectionnée pour l’unité publicitaire comporte au moins une soumission qui a atteint [dans le magasin de](../publish/the-app-certification-process.md#in-the-store) l’étape dans l’application processus de certification.

Dans le cadre de la réglementation COPPA (Children's Online Privacy Protection Act), vous devez sélectionner **Cette application est destinée à des enfants de moins de 13** dans cette section si votre application s’adresse aux enfants de moins de 13ans. Si vous sélectionnez cette option, Microsoft prendra les mesures nécessaires pour désactiver les services de publicité comportementale lors de la diffusion de publicités dans votre application.

Le paramètre **Conformité avec la réglementation COPPA** que vous choisissez est automatiquement appliqué à toutes les unités publicitaires de l’application sélectionnée.

> [!IMPORTANT]
> Si votre application s’adresse aux enfants de moins de 13ans, la réglementation COPPA vous impose certaines obligations. Pour plus d’informations sur les obligations qui vous incombent, veuillez consulter [cette page](http://go.microsoft.com/fwlink/p/?linkid=536558).
