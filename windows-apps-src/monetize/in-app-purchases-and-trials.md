---
author: Xansky
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Découvrez comment activer les versions d’évaluation et achats in-app dans les applicationsUWP.
title: Achats dans l’application et versions d’évaluation
ms.author: mhopkins
ms.date: 05/09/2018
ms.topic: article
keywords: windows10, uwp, achats dans l’application, extensions, versions d’évaluation, consommables, durables, abonnement
ms.localizationpriority: medium
ms.openlocfilehash: 2c1c4ea1923ff81754b9c8ed8328ba6ec670a3f1
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7146954"
---
# <a name="in-app-purchases-and-trials"></a>Achats dans l’application et versions d’évaluation

Le WindowsSDK fournit des API que vous pouvez utiliser pour implémenter les fonctionnalités suivantes afin de générer davantage de revenus avec votre applicationUWP:

* **Achats in-app**&nbsp;&nbsp;Que votre application soit gratuite ou non, vous pouvez vendre du contenu ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application.

* **Fonctionnalités d’évaluation**&nbsp;&nbsp;si vous [Configurez votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial), vous pouvez encourager vos clients à acheter la version complète de votre application en excluant ou en limitant certaines fonctionnalités durant la période d’évaluation. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article vous fournit une vue d’ensemble du fonctionnement des achats in-app et des versions d’évaluation dans les applicationsUWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Choisir l’espace de noms à utiliser

Il existe deuxespaces de noms différents à utiliser pour ajouter des achats in-app et des fonctionnalités de version d’évaluation dans vos applicationsUWP, en fonction de la version de Windows10 ciblée par vos applications. Bien que les API dans ces espaces de noms servent les mêmes objectifs, elles sont conçues légèrement différemment et leur code n’est pas compatible.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;À partir de Windows10, version1607, les applications peuvent utiliser l’API dans cet espace de noms pour implémenter les achats in-app et les versions d’évaluation. Nous vous recommandons d’utiliser les membres dans cet espace de noms si votre projet d’application cible **Windows10 Anniversary Edition (version10.0; build 14393)** ou une version ultérieure dans Visual Studio. Cet espace de noms prend en charge les types d’extension plus récents, comme les extensions consommables gérés par le Windows Store et est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par l’espace partenaires et le Windows Store. Pour plus d’informations sur cet espace de noms, consultez la section [Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store](#api_intro) de cet article.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;L’ensemble des versions de Windows10 prennent également en charge une API plus ancienne pour les achats in-app et les versions d’évaluation dans cet espace de noms. Si vous recherchez des informations sur l’espace de noms **Windows.ApplicationModel.Store**, consultez la page [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> L'espace de noms **Windows.ApplicationModel.Store** n’est plus mis à jour avec de nouvelles fonctionnalités et nous vous recommandons d’utiliser l'espace de noms **Windows.Services.Store** pour votre app à la place, si cela est possible. L’espace de noms **Windows.ApplicationModel.Store** n’est pas pris en charge dans les applications de bureau Windows qui utilisent le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop) ou dans des applications ou jeux qui utilisent un bac à sable de développement dans l’espace partenaires (par exemple, c’est le cas pour les jeux qui s’intègre à Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Concepts de base

En général, chacun des éléments proposés dans le WindowsStore est appelé *produit*. La plupart des développeurs utilisent uniquement les types de produit suivants: *applications* et *extensions*.

Une extension est un produit ou une fonctionnalité que vous mettez à la disposition de vos clients dans le contexte de votre application: par exemple, la devise à utiliser dans une application ou un jeu, les nouvelles cartes ou armes d’un jeu, la possibilité d’utiliser votre application sans publicité, ou le contenu numérique comme de la musique ou des vidéos pour les applications qui peuvent proposer ce type de contenu. Chaque application ou module complémentaire a une licence qui indique si l’utilisateur est autorisé à l’utiliser. Si l’utilisateur est autorisé à utiliser l’application ou le module complémentaire en tant que version d’évaluation, la licence fournit également des informations supplémentaires sur la version d’évaluation.

Pour proposer un module complémentaire aux clients de votre application, vous devez [définir le module complémentaire pour votre application dans l’espace partenaires](../publish/add-on-submissions.md) afin de signaler cette information au Windows Store. Ensuite, votre application peut utiliser les API des espaces de noms **Windows.Services.Store** ou **Windows.ApplicationModel.Store** pour proposer le module complémentaire à la vente à l’utilisateur, en tant qu’achat in-app.

Les applications UWP peuvent offrir des types de modules complémentaires suivants.

| Type de module complémentaire |  Description  |
|---------|-------------------|
| Durable  |  Un module complémentaire conservé pendant la durée de vie que vous [spécifiez dans l’espace partenaires](../publish/enter-iap-properties.md). <p/><p/>Par défaut, les modules complémentaires durables n’ont pas de date d’expiration et ne peuvent donc être achetés qu’une seule fois. Si vous spécifiez une durée pour le module complémentaire, l’utilisateur peut racheter le module complémentaire après l’expiration de ce dernier. |
| Consommable géré par le développeur  |  Une extension qu’il est possible d’acheter, d’utiliser, puis d’acheter à nouveau après sa consommation. Vous êtes responsable du suivi du solde d’éléments de l’utilisateur, que l'extension représente.<p/><p/>Quand l'utilisateur consomme des éléments associés à l'extension, vous devez gérer le solde de l’utilisateur et signaler l’achat de l'extension comme épuisé dans le Store une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé. <p/><p/>Par exemple, si votre module complémentaire représente 100pièces dans un jeu et que l’utilisateur en consomme10, votre application ou votre service doit maintenir le nouveau solde restant de 90pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100pièces.    |
| Consommable géré par le Store  |  Extension qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau à tout moment. Le Store gère le solde d’éléments de l’utilisateur, que l'extension représente.<p/><p/>Lorsque l’utilisateur utilise des éléments associés à l'extension, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. L’utilisateur peut acheter l'extension autant de fois qu’il le souhaite (il est inutile de consommer les éléments d’abord). Votre application peut demander le solde actuel de l’utilisateur à tout moment. <p/><p/> Par exemple, si votre extension représente une quantité initiale de 100pièces dans un jeu et que l’utilisateur consomme 50pièces, votre application signale au Store que 50unités de l'extension ont été consommées, et le Store met à jour le solde restant. Si l’utilisateur rachète ensuite votre extension pour obtenir 100pièces de plus, il possède alors 150pièces au total. <p/><p/>**Remarque**&nbsp;&nbsp;Pour utiliser les consommables gérés par le Store, votre application doit cibler **Windows10, Anniversary Edition (10.0; Build14393)** ou une version ultérieure dans Visual Studio et utiliser l’espace de noms **Windows.Services.Store** à la place de l’espace de noms **Windows.ApplicationModel.Store**.  |
| Abonnement | Extension durable dans laquelle le client continue à être facturé à intervalles réguliers pour continuer à utiliser l'extension. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. <p/><p/>**Remarque**&nbsp;&nbsp;Pour utiliser les extensions d'abonnement, votre application doit cibler **Windows10 Anniversary Edition (10.0; Build14393)** ou une version ultérieure dans Visual Studio et utiliser l’espace de noms **Windows.Services.Store** à la place de l’espace de noms **Windows.ApplicationModel.Store**.  |

<span />

> [!NOTE]
> D’autres types d’extension, comme les extensions durables avec des packages (également appelés contenu téléchargeable ou DLC) ne sont disponibles que pour un groupe restreint de développeurs et ne sont pas traités dans cette documentation.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store

Cette section fournit une vue d’ensemble des tâches et concepts importants de l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Cet espace de noms est disponible uniquement pour les applications qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio (ce qui correspond à Windows10, version1607). Nous recommandons l’utilisation de l'espace de noms **Windows.Services.Store** à la place de l'espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx), si possible. Pour plus d’informations sur l’espace de noms **Windows.ApplicationModel.Store**, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**Dans cette section**

* [Vidéo](#video)
* [Prise en main de la classe StoreContext](#get-started-storecontext)
* [Implémenter des achats dans l’application](#implement-iap)
* [Implémenter les fonctionnalités des versions d’évaluation](#implement-trial)
* [Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation](#testing)
* [Reçus des achats dans l’application](#receipts)
* [Utilisation de la classe StoreContext avec le Pont du bureau](#desktop)
* [Produits, références et disponibilités](#products-skus)
* [ID Store](#store-ids)

<span id="video" />

### <a name="video"></a>Vidéo

Regardez la vidéo suivante pour une vue d’ensemble de l’implémentation des achats in-app dans votre application à l'aide de l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Prise en main de la classe StoreContext

Le point d’entrée principal de l’espace de noms **Windows.Services.Store** est la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Cette classe fournit des méthodes que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses modules complémentaires disponibles, acheter une application ou un module complémentaire pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses modules complémentaires. Pour obtenir un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), effectuez l’une des opérations suivantes:

* Dans une app mono-utilisateur (c’est-à-dire une app qui ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée), utilisez la méthode statique [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du MicrosoftStore concernant l’utilisateur. La plupart des applications de plateforme Windows universelle (UWP) sont des applications mono-utilisateur.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Dans une [app multi-utilisateur](../xbox-apps/multi-user-applications.md), utilisez la méthode statique [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du MicrosoftStore concernant un utilisateur connecté avec son compte Microsoft pendant qu’il utilise l’app. L’exemple suivant obtient un objet **StoreContext** pour le premier utilisateur disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Les applications de bureau Windows qui utilisent le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop) doivent exécuter des tâches supplémentaires de configuration de l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) avant de pouvoir l’utiliser. Pour plus d’informations, consultez [cette section](#desktop).

Une fois que vous possédez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), vous pouvez commencer à appeler des méthodes de cet objet pour obtenir des informations produit du WindowsStore pour l’application actuelle et ses extensions, récupérer des informations de licence de l’application actuelle et de ses extensions, acheter une application ou une extension pour l’utilisateur actuel, et effectuer d’autres tâches. Pour plus d’informations sur les tâches courantes que vous pouvez exécuter à l’aide de cet objet, consultez les articles suivants:

* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour consulter un exemple d’application illustrant l’utilisation de l’objet **StoreContext** et d’autres types dans l’espace de noms **Windows.Services.Store**, consultez [Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implémenter des achats dans l’application

Pour proposer un achat dans l’application aux clients de votre application à l’aide de l’espace de noms **Windows.Services.Store**:

1. Si votre application propose des modules complémentaires que les clients peuvent acheter, [créer des soumissions de module complémentaire pour votre application dans l’espace partenaires ](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit pour votre application ou un module complémentaire offert par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence est active](get-license-info-for-apps-and-add-ons.md) (c’est-à-dire, si l’utilisateur dispose d’une licence lui permettant d’utiliser l’application ou le module complémentaire). Si la licence n’est pas active, affichez une interface utilisateur qui propose à l’utilisateur l’application ou le module complémentaire à la vente en tant qu’achat in-app.

3. Si l’utilisateur choisit d’acheter votre application ou extension, utilisez la méthode d’achat appropriée du produit:

    * Si l’utilisateur achète votre application ou une extension durable, suivez les instructions de la rubrique [Activer les achats in-app d’applications et d’extensions](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Si l’utilisateur achète une extension consommable, suivez les instructions de la rubrique [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md).
    * Si l’utilisateur achète une extension d'abonnement, suivez les instructions de la rubrique [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implémenter les fonctionnalités des versions d’évaluation

Pour exclure ou limiter les fonctionnalités d’une version d’évaluation de votre application à l’aide de l’espace de noms **Windows.Services.Store**:

1. [Configurer votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit de votre application et d’un module complémentaire pris en charge par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence associée à l’application est une licence d’évaluation](get-license-info-for-apps-and-add-ons.md).

3. Excluez ou limitez certaines fonctionnalités de votre application s’il s’agit d’une version d’évaluation, puis activez les fonctionnalités quand l’utilisateur acquiert une licence complète. Pour obtenir plus d’informations et un exemple de code, voir [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation

Si votre application utilise des API de l'espace de noms **Windows.Services.Store** pour implémenter des achats in-app et la fonctionnalité de version d’évaluation, vous devez la publier dans le Store et télécharger l’application sur votre appareil de développement pour utiliser sa licence à des fins de test. Procédez comme suit pour tester votre code:

1. Si votre application n’est pas encore publiée et disponible dans le Windows Store, assurez-vous que votre application minimale requise [Kit de Certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) , de [soumettre votre application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans l’espace partenaires et assurez-vous que votre application réussit le processus de certification. Vous pouvez [configurer votre application pour qu'elle ne soit pas détectable dans le Windows Store](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) pendant que vous la testez. Notez la configuration correcte de [versions d’évaluation de package](../publish/package-flights.md). Incorrectement package configuré, versions d’évaluation est peut-être ne pas en mesure d’être téléchargées.

2. Ensuite, assurez-vous d’exécuter les actions suivantes:

    * Écrivez du code dans votre application qui utilise la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) et d’autres types associés dans l’espace de noms **Windows.Services.Store** afin d’implémenter les [achats dans l’application](#implement-iap) ou une [fonctionnalité de version d’évaluation](#implement-trial).
    * Si votre application propose un module complémentaire que les clients peuvent acheter, [créez une soumission d’extension pour votre application dans l’espace partenaires](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Si vous souhaitez exclure ou limiter certaines fonctionnalités dans une version d’évaluation de votre application, [Configurez votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

3. Avec votre projet ouvert dans Visual Studio, cliquez sur le **menu Projet**, pointez sur **Store**, puis cliquez sur **Associer l’application au WindowsStore**. Suivez les instructions de l’Assistant pour associer le projet d’application à l’application dans votre compte du centre de l’espace que vous souhaitez utiliser pour le test.
    > [!NOTE]
    > Si vous n’associez pas votre projet à une application du Windows Store, les méthodes [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) attribuent le code d’erreur0x803F6107 à la propriété **ExtendedError** dans la valeur renvoyée. Cette valeur indique que le WindowsStore ne connaît pas l’application.
4. Si ce n’est déjà fait, installez l’application à partir du WindowsStore que vous avez spécifié à l’étape précédente, exécutez l’application une fois, puis fermez-la. Ceci garantit l’installation d’une licence valide de l’application sur votre appareil de développement.

5. Dans VisualStudio, exécutez ou déboguez votre projet. Votre code doit récupérer les données d’application et de module complémentaire, à partir de l’application WindowsStore que vous avez associée à votre projet local. Si vous êtes invité à réinstaller l’application, suivez les instructions, puis exécutez ou déboguez votre projet.
    > [!NOTE]
    > Après avoir effectué ces étapes, vous pouvez continuer et mettre à jour le code de votre application, puis déboguer votre projet mis à jour sur votre ordinateur de développement sans soumettre de nouveaux packages d’application au WindowsStore. Il vous suffit de télécharger la version WindowsStore de votre application sur votre ordinateur de développement pour obtenir la licence locale qui sera utilisée pour les tests. Vous devez uniquement soumettre les nouveaux packages d’application au WindowsStore après avoir terminé vos tests et proposer à vos clients les fonctionnalités liées aux achats in-app ou à la version d’évaluation de votre application.

Si votre application utilise l'espace de noms **Windows.ApplicationModel.Store**, vous pouvez utiliser la classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) dans votre application pour simuler les informations de licence pendant le test, avant de soumettre votre application au WindowsStore. Pour plus d’informations, voir [Prise en main des classes CurrentApp et CurrentAppSimulator] (in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. Si vous utilisez l'espace de noms **Windows.Services.Store** pour implémenter les achats dans l'application ou les versions d’évaluation, vous devez publier votre application dans le Windows Store et télécharger l’application sur votre appareil de développement pour utiliser sa licence à des fins de test, comme indiqué ci-dessus.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Reçus des achats dans l’application

L’espace de noms **Windows.Services.Store** ne fournit aucune API vous permettant d’obtenir un reçu de transaction pour les achats effectués dans le code de votre application. Il s’agit d’une expérience différente de celles des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, qui peuvent elles [utiliser une API côté client pour récupérer un reçu de transaction](use-receipts-to-verify-product-purchases.md).

Si vous implémentez des achats dans l’application à l’aide de l’espace de noms **Windows.Services.Store** et que vous souhaitez confirmer l’achat par le client d’une app ou d’un module complémentaire, vous pouvez utiliser la [méthode de requête de produits](query-for-products.md) dans l’[API REST de collection du MicrosoftStore](view-and-grant-products-from-a-service.md). Les données renvoyées avec cette méthode confirment si l’utilisateur considéré possède des droits pour un produit donné, et fournissent des données pour la transaction dans laquelle l’utilisateur a acquis le produit. L’API de collection du MicrosoftStore utilise l’authentificationAzureAD pour récupérer ces informations.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Utilisation de la classe StoreContext avec DesktopBridge

Les applications de bureau qui utilisent [DesktopBridge](https://developer.microsoft.com/windows/bridges/desktop) peuvent utiliser la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour implémenter des achats dans l’application et des versions d’évaluation. Toutefois, si vous possédez une application de bureau Win32 ou une application de bureau présentant un handleWindows associé à l’infrastructure de rendu (comme une application WPF), votre application doit configurer l’objet **StoreContext** pour identifier la fenêtre applicative du propriétaire pour les boîtes de dialogue modales affichées par l’objet.

De nombreux membres **StoreContext** (et des membres d’autres types associés accessibles via l’objet **StoreContext**) affichent une boîte de dialogue modale à l’utilisateur, pour les opérations associées au WindowsStore telle que l’achat d’un produit. Si une application de bureau ne configure pas l’objet **StoreContext** afin de spécifier la fenêtre du propriétaire pour les boîtes de dialogue modales, cet objet renvoie des données inappropriées ou des erreurs.

Pour configurer un objet **StoreContext** dans une application de bureau qui utilise DesktopBridge, procédez comme suit.

1. Pour permettre à votre application d’accéder à l’interface [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx), effectuez l’une des actions suivantes:

    * Si votre application est écrite dans un langage géré comme C# ou VisualBasic, déclarez l’interface **IInitializeWithWindow** dans le code de votre application avec l’attribut [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx), comme représenté dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction **using** pour l’espace de noms **System.Runtime.InteropServices**.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * Si votre application est écrite en C++, ajoutez une référence au fichier d’en-tête shobjidl.h dans votre code. Ce fichier d’en-tête contient la déclaration de l’interface **IInitializeWithWindow**.

2. Récupérez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en utilisant la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) (ou la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) si votre application est une [application multi-utilisateur](../xbox-apps/multi-user-applications.md)) comme décrit plus haut dans cet article, puis convertissez cet objet en objet [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx), puis transmettez le handle de la fenêtre que vous souhaitez configurer comme propriétaire pour toutes les boîtes de dialogue modales affichées par les méthodes **StoreContext**. L’exemple suivant en C# vous explique comment transmettre le handle de la fenêtre principale de votre application à la méthode.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Produits, références et disponibilités

Chaque produit dans le WindowsStore a au moins une *référence (SKU)*, et chaque référence (SKU) a au moins une *disponibilité*. Ces concepts sont employés par la plupart des développeurs dans l’espace partenaires, et la plupart des développeurs ne définissent jamais les références (SKU) ou les disponibilités de leurs applications ou des modules complémentaires. Toutefois, comme le modèle d’objet des produits du WindowsStore dans l’espace de noms **Windows.Services.Store** contient des références (SKU) et des disponibilités, la compréhension de ces concepts de base peut être utile pour certains scénarios.

| Objet |  Description  |
|---------|-------------------|
| Produit  |  Un *produit* désigne un type de produit disponible dans le WindowsStore, notamment une application ou une extension. <p/><p/> Chaque produit dans le Windows Store est associé à un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Cette classe fournit des propriétés qui vous permettent d’accéder à des données, telles que l’ID WindowsStore du produit, les images et vidéos de la liste du WindowsStore, ainsi que des informations tarifaires. Elle fournit également des méthodes que vous pouvez utiliser pour acheter le produit. |
| SKU |  Une *référence (SKU)* est une version spécifique d’un produit avec sa propre description, son prix et d’autres détails uniques sur le produit. Chaque application ou module complémentaire a une référence (SKU) par défaut. Le seul cas où la plupart des développeurs ont plusieurs références (SKU) pour une application, c’est lorsqu’ils publient une version complète de leur application et une version d’évaluation. Dans le catalogue du WindowsStore, chacune de ces versions a une référence (SKU) différente de la même application. <p/><p/> Certains éditeurs peuvent définir leurs propres références (SKU). Par exemple, un grand éditeur de jeux peut commercialiser un jeu avec une référence (SKU) qui affiche du sang vert sur les marchés qui n’autorisent pas le sang rouge et une autre référence (SKU) montrant du sang rouge sur tous les autres marchés. Autre possibilité, un éditeur qui vend des contenus vidéo numériques peut publier deuxréférences (SKU) pour une vidéo: une pour la version haute définition et une autre pour la version en définition standard. <p/><p/> Chaque référence (SKU) dans le Windows Store est associé à un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx). Chaque [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) a une propriété [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) que vous pouvez utiliser pour accéder aux références (SKU) du produit. |
| Disponibilité  |  Une *disponibilité* est une version spécifique d’une référence (SKU) avec des informations de tarification qui lui sont propres. Chaque référence (SKU) a une disponibilité par défaut. Certains éditeurs peuvent définir leurs propres disponibilités pour proposer des prix différents pour une référence (SKU) donnée. <p/><p/> Chaque disponibilité dans le Windows Store est associée à un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx). Chaque [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) a une propriété [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) que vous pouvez utiliser pour accéder aux disponibilités de la référence (SKU). Pour la plupart des développeurs, chaque référence (SKU) a une disponibilité unique par défaut.  |

<span id="store_ids" />

### <a name="store-ids"></a>ID Store

Chaque application, extension ou autre produit dans le Windows Store est associé à un **ID Store** (parfois également appelé *ID Store du produit*). Un grand nombre d’API requièrent l’ID Store pour effectuer une opération sur une application ou une extension.

L’ID Store d’un produit dans le WindowsStore est composé de 12caractères alphanumériques, comme ```9NBLGGH4R315```. Il existe différentes méthodes pour obtenir l’ID Store d'un produit dans le Windows Store:

* Pour une application, vous pouvez obtenir l’ID Windows Store sur la [page identité de l’application](../publish/view-app-identity-details.md) dans l’espace partenaires.
* Pour une extension, vous pouvez obtenir l’ID Windows Store sur le de vue d’ensemble page Ajout dans l’espace partenaires.
* Pour un produit, vous pouvez également obtenir l’ID Store par programme à l’aide de la propriété [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) de l'objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représente le produit.

Pour les produits dotés de références (SKU) et de disponibilités, les références (SKU) et les disponibilités ont également leurs propres ID Windows Store avec différents formats.

| Objet |  Format de l’ID Store  |
|---------|-------------------|
| SKU |  L’ID Store d'une référence (SKU) présente le format ```<product Store ID>/xxxx```, où ```xxxx``` est une chaîne alphanumérique de 4caractères qui identifie une référence (SKU) du produit. Exemple: ```9NBLGGH4R315/000N```. Cet ID est retourné par la propriété [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) d’un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) et il est parfois appelé *ID Store de référence (SKU)*. |
| Disponibilité  |  L’ID Store d'une disponibilité a le format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, où ```xxxx``` est une chaîne alphanumérique de 4caractères qui identifie une référence (SKU) du produit et ```yyyyyyyyyyyy``` est une chaîne alphanumérique de 12caractères qui identifie une disponibilité de la référence (SKU). Exemple: ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Cet ID est retourné par la propriété [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) d’un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) et il est parfois appelé *ID Store de disponibilité*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Comment utiliser l’ID produit pour les extensions dans votre code

Si vous souhaitez rendre une extension disponible pour vos clients dans le contexte de votre application, vous devez [entrer un ID de produit unique](../publish/set-your-add-on-product-id.md#product-id) pour votre extension lorsque vous [créez votre soumission d’extension](../publish/add-on-submissions.md) dans l’espace partenaires. Vous pouvez utiliser cet ID produit pour faire référence à l'extension dans votre code, bien que les scénarios spécifiques dans lesquels vous pouvez utiliser l’ID produit dépendent de l’espace de noms que vous utilisez pour les achats in-app dans votre application.

> [!NOTE]
> L’ID de produit que vous entrez dans l’espace partenaires pour une extension est différente de celle de l' [ID Windows Store ajouter](#store-ids). L’ID Windows Store est généré par l’espace partenaires.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.Services.Store

Si votre application utilise l'espace de noms **Windows.Services.Store**, vous pouvez utiliser l’ID produit pour identifier facilement le [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) qui représente votre extension ou le [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) qui représente la licence de votre extension. L'ID produit est exposé par les propriétés [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) et [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken).

> [!NOTE]
> Bien que l’ID produit soit un moyen utile pour identifier une extension dans votre code, la plupart des opérations dans l’espace de noms **Windows.Services.Store** utilisent l'[ID Store](#store-ids) d’une extension au lieu de l’ID produit. Par exemple, pour récupérer par programme une ou plusieurs extensions connues pour une application, passez les ID Store (plutôt que les ID produit) des extensions à la méthode [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). De même, pour signaler une extension consommable comme épuisée, passez l’ID Store de l’extension (plutôt que l’ID produit) à la méthode [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.ApplicationModel.Store

Si votre application utilise l’espace de noms **Windows.ApplicationModel.Store** , vous devez utiliser l’ID de produit que vous attribuez à un module complémentaire dans l’espace partenaires pour la plupart des opérations. Exemple:

* Utilisez l’ID produit pour identifier le [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) qui représente votre extension ou le [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) qui représente la licence de votre extension. L'ID produit est exposé par les propriétés [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) et [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId).

* Spécifiez l’ID produit lorsque vous achetez votre extension pour l’utilisateur à l’aide de la méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync). Pour plus d’informations, voir [Activer l’achat de produits dans l’application](enable-in-app-product-purchases.md).

* Spécifiez l’ID produit lorsque vous signalez votre extension consommable comme épuisée à l’aide de la méthode [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync). Pour plus d’informations, voir [Activer l’achat de produits dans l'application consommables](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Rubriques associées

* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Codes d’erreur pour les opérations du Store](error-codes-for-store-operations.md)
* [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
