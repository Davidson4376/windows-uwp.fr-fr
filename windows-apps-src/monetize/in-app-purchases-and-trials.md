---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Découvrez comment activer les versions d’évaluation et achats in-app dans les applications UWP.
title: Versions d’évaluation et achats in-app
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, achats dans l’application, extensions, versions d’évaluation, consommables, durables, abonnement
ms.localizationpriority: medium
ms.openlocfilehash: 5a8bad5207c5907beb91e5664b4bc7e140ab036b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656954"
---
# <a name="in-app-purchases-and-trials"></a>Versions d’évaluation et achats in-app

Le Windows SDK fournit des API que vous pouvez utiliser pour implémenter les fonctionnalités suivantes afin de générer davantage de revenus avec votre application UWP :

* **Achats in-app**&nbsp;&nbsp;Que votre application soit gratuite ou non, vous pouvez vendre du contenu ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application.

* **Fonctionnalités d’évaluation**&nbsp;&nbsp;si vous [configurer votre application en tant qu’une version d’évaluation gratuite dans partenaires](../publish/set-app-pricing-and-availability.md#free-trial), vous pouvez inciter vos clients à acheter la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article vous fournit une vue d’ensemble du fonctionnement des achats in-app et des versions d’évaluation dans les applications UWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Choisir l’espace de noms à utiliser

Il existe deux espaces de noms différents à utiliser pour ajouter des achats in-app et des fonctionnalités de version d’évaluation dans vos applications UWP, en fonction de la version de Windows 10 ciblée par vos applications. Bien que les API dans ces espaces de noms servent les mêmes objectifs, elles sont conçues légèrement différemment et leur code n’est pas compatible.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;à compter de Windows 10, version 1607, applications peuvent utiliser l’API dans cet espace de noms pour implémenter des achats dans l’application et les versions d’évaluation. Nous vous recommandons d’utiliser les membres dans cet espace de noms si votre projet d’application cible **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Cet espace de noms prend en charge les derniers types de module complémentaire, tels que des modules complémentaires consommables gérés par le Store et est conçu pour être compatibles avec les types futures des produits et fonctionnalités pris en charge par les partenaires et le Store. Pour plus d’informations sur cet espace de noms, consultez la section [Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store](#api_intro) de cet article.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;toutes les versions de Windows 10 prennent également en charge une ancienne API pour les achats dans l’application et les versions d’évaluation dans cet espace de noms. Si vous recherchez des informations sur l’espace de noms **Windows.ApplicationModel.Store**, consultez la page [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> L'espace de noms **Windows.ApplicationModel.Store** n’est plus mis à jour avec de nouvelles fonctionnalités et nous vous recommandons d’utiliser l'espace de noms **Windows.Services.Store** pour votre app à la place, si cela est possible. Le **Windows.ApplicationModel.Store** espace de noms n’est pas pris en charge dans les applications de bureau Windows qui utilisent la [pont du bureau](https://developer.microsoft.com/windows/bridges/desktop) ou dans les applications ou les jeux qui utilisent un bac à sable de développement dans le centre de partenaires (pour exemple, c’est le cas pour les jeux qui s’intègre à Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Concepts de base

En général, chacun des éléments proposés dans le Windows Store est appelé *produit*. La plupart des développeurs utilisent uniquement les types de produit suivants : *applications* et *extensions*.

Une extension est un produit ou une fonctionnalité que vous mettez à la disposition de vos clients dans le contexte de votre application : par exemple, la devise à utiliser dans une application ou un jeu, les nouvelles cartes ou armes d’un jeu, la possibilité d’utiliser votre application sans publicité, ou le contenu numérique comme de la musique ou des vidéos pour les applications qui peuvent proposer ce type de contenu. Chaque application ou module complémentaire a une licence qui indique si l’utilisateur est autorisé à l’utiliser. Si l’utilisateur est autorisé à utiliser l’application ou le module complémentaire en tant que version d’évaluation, la licence fournit également des informations supplémentaires sur la version d’évaluation.

Pour vous proposer un module complémentaire aux clients dans votre application, vous devez [définissent le module complémentaire pour votre application dans le centre partenaires](../publish/add-on-submissions.md) pour que le Store sache à son sujet. Ensuite, votre application peut utiliser les API des espaces de noms **Windows.Services.Store** ou **Windows.ApplicationModel.Store** pour proposer le module complémentaire à la vente à l’utilisateur, en tant qu’achat in-app.

Les applications UWP peuvent offrir des types de modules complémentaires suivants.

| Type de module complémentaire |  Description  |
|---------|-------------------|
| Durable  |  Un module complémentaire qui persiste pendant la durée de vie que vous avez [spécifier dans le centre partenaires](../publish/enter-iap-properties.md). <p/><p/>Par défaut, les modules complémentaires durables n’ont pas de date d’expiration et ne peuvent donc être achetés qu’une seule fois. Si vous spécifiez une durée pour le module complémentaire, l’utilisateur peut racheter le module complémentaire après l’expiration de ce dernier. |
| Consommable géré par le développeur  |  Une extension qu’il est possible d’acheter, d’utiliser, puis d’acheter à nouveau après sa consommation. Vous êtes responsable du suivi du solde d’éléments de l’utilisateur, que l'extension représente.<p/><p/>Quand l'utilisateur consomme des éléments associés à l'extension, vous devez gérer le solde de l’utilisateur et signaler l’achat de l'extension comme épuisé dans le Store une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé. <p/><p/>Par exemple, si votre module complémentaire représente 100 pièces dans un jeu et que l’utilisateur en consomme 10, votre application ou votre service doit maintenir le nouveau solde restant de 90 pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100 pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100 pièces.    |
| Consommable géré par le Windows Store  |  Extension qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau à tout moment. Le Store gère le solde d’éléments de l’utilisateur, que l'extension représente.<p/><p/>Lorsque l’utilisateur utilise des éléments associés à l'extension, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. L’utilisateur peut acheter l'extension autant de fois qu’il le souhaite (il est inutile de consommer les éléments d’abord). Votre application peut demander le solde actuel de l’utilisateur à tout moment. <p/><p/> Par exemple, si votre extension représente une quantité initiale de 100 pièces dans un jeu et que l’utilisateur consomme 50 pièces, votre application signale au Store que 50 unités de l'extension ont été consommées, et le Store met à jour le solde restant. Si l’utilisateur rachète ensuite votre extension pour obtenir 100 pièces de plus, il possède alors 150 pièces au total. <p/><p/>**Remarque**&nbsp;&nbsp;Pour utiliser les consommables gérés par le Store, votre application doit cibler **Windows 10, Anniversary Edition (10.0; Build 14393)** ou une version ultérieure dans Visual Studio et utiliser l’espace de noms **Windows.Services.Store** à la place de l’espace de noms **Windows.ApplicationModel.Store**.  |
| Abonnement | Extension durable dans laquelle le client continue à être facturé à intervalles réguliers pour continuer à utiliser l'extension. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. <p/><p/>**Remarque**&nbsp;&nbsp;Pour utiliser les extensions d'abonnement, votre application doit cibler **Windows 10 Anniversary Edition (10.0 ; Build 14393)** ou une version ultérieure dans Visual Studio et utiliser l’espace de noms **Windows.Services.Store** à la place de l’espace de noms **Windows.ApplicationModel.Store**.  |

<span />

> [!NOTE]
> D’autres types d’extension, comme les extensions durables avec des packages (également appelés contenu téléchargeable ou DLC) ne sont disponibles que pour un groupe restreint de développeurs et ne sont pas traités dans cette documentation.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store

Cette section fournit une vue d’ensemble des tâches et concepts importants de l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Cet espace de noms est disponible uniquement pour les applications qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio (ce qui correspond à Windows 10, version 1607). Nous recommandons l’utilisation de l'espace de noms **Windows.Services.Store** à la place de l'espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx), si possible. Pour plus d’informations sur l’espace de noms **Windows.ApplicationModel.Store**, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**Dans cette section**

* [Vidéo](#video)
* [Bien démarrer avec la classe StoreContext](#get-started-storecontext)
* [Implémenter des achats dans l’application](#implement-iap)
* [Implémenter la fonctionnalité d’évaluation](#implement-trial)
* [Tester votre achat dans l’application ou l’implémentation d’évaluation](#testing)
* [Accusés de réception pour les achats dans l’application](#receipts)
* [À l’aide de la classe StoreContext avec le pont du bureau](#desktop)
* [Produits, des références (SKU) et des disponibilités](#products-skus)
* [ID de Store](#store-ids)

<span id="video" />

### <a name="video"></a>Video

Regardez la vidéo suivante pour une vue d’ensemble de l’implémentation des achats in-app dans votre application à l'aide de l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Démarrer avec la classe StoreContext

Le point d’entrée principal de l’espace de noms **Windows.Services.Store** est la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Cette classe fournit des méthodes que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses modules complémentaires disponibles, acheter une application ou un module complémentaire pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses modules complémentaires. Pour obtenir un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), effectuez l’une des opérations suivantes :

* Dans une app mono-utilisateur (c’est-à-dire une app qui ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée), utilisez la méthode statique [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du Microsoft Store concernant l’utilisateur. La plupart des applications de plateforme Windows universelle (UWP) sont des applications mono-utilisateur.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Dans une [app multi-utilisateur](../xbox-apps/multi-user-applications.md), utilisez la méthode statique [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du Microsoft Store concernant un utilisateur connecté avec son compte Microsoft pendant qu’il utilise l’app. L’exemple suivant obtient un objet **StoreContext** pour le premier utilisateur disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Les applications de bureau Windows qui utilisent le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop) doivent exécuter des tâches supplémentaires de configuration de l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) avant de pouvoir l’utiliser. Pour plus d’informations, consultez [cette section](#desktop).

Une fois que vous possédez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), vous pouvez commencer à appeler des méthodes de cet objet pour obtenir des informations produit du Windows Store pour l’application actuelle et ses extensions, récupérer des informations de licence de l’application actuelle et de ses extensions, acheter une application ou une extension pour l’utilisateur actuel, et effectuer d’autres tâches. Pour plus d’informations sur les tâches courantes que vous pouvez exécuter à l’aide de cet objet, consultez les articles suivants :

* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Activer les modules complémentaires d’abonnement pour votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour consulter un exemple d’application illustrant l’utilisation de l’objet **StoreContext** et d’autres types dans l’espace de noms **Windows.Services.Store**, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implémenter des achats dans l’application

Pour proposer un achat dans l’application aux clients de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. Si votre application propose des modules complémentaires que les clients peuvent acheter, [créer des soumissions de module complémentaire pour votre application dans partenaires ](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit pour votre application ou un module complémentaire offert par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence est active](get-license-info-for-apps-and-add-ons.md) (c’est-à-dire, si l’utilisateur dispose d’une licence lui permettant d’utiliser l’application ou le module complémentaire). Si la licence n’est pas active, affichez une interface utilisateur qui propose à l’utilisateur l’application ou le module complémentaire à la vente en tant qu’achat in-app.

3. Si l’utilisateur choisit d’acheter votre application ou extension, utilisez la méthode d’achat appropriée du produit :

    * Si l’utilisateur achète votre application ou une extension durable, suivez les instructions de la rubrique [Activer les achats in-app d’applications et d’extensions](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Si l’utilisateur achète un module complémentaire de consommable, suivez les instructions de la rubrique [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md).
    * Si l’utilisateur achète une extension d'abonnement, suivez les instructions de la rubrique [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implémenter les fonctionnalités des versions d’évaluation

Pour exclure ou limiter les fonctionnalités d’une version d’évaluation de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. [Configurer votre application en tant qu’une version d’évaluation gratuite dans partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit de votre application et d’un module complémentaire pris en charge par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence associée à l’application est une licence d’évaluation](get-license-info-for-apps-and-add-ons.md).

3. Excluez ou limitez certaines fonctionnalités de votre application s’il s’agit d’une version d’évaluation, puis activez les fonctionnalités quand l’utilisateur acquiert une licence complète. Pour obtenir plus d’informations et un exemple de code, voir [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation

Si votre application utilise des API de l'espace de noms **Windows.Services.Store** pour implémenter des achats in-app et la fonctionnalité de version d’évaluation, vous devez la publier dans le Store et télécharger l’application sur votre appareil de développement pour utiliser sa licence à des fins de test. Procédez comme suit pour tester votre code :

1. Si votre application n’est pas encore publié et disponible dans le Store, assurez-vous que votre application répond à la valeur minimale [Kit de Certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) exigences, [soumettre votre application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans partenaires et assurez-vous que votre application transmet le processus de certification. Vous pouvez [configurer votre application pour qu'elle ne soit pas détectable dans le Windows Store](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) pendant que vous la testez. Veuillez noter la configuration adéquate des [package vols](../publish/package-flights.md). Incorrectement configuré package vols est peut-être ne pas être en mesure d’être téléchargé.

2. Ensuite, assurez-vous d’exécuter les actions suivantes :

    * Écrivez du code dans votre application qui utilise la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) et d’autres types associés dans l’espace de noms **Windows.Services.Store** afin d’implémenter les [achats dans l’application](#implement-iap) ou une [fonctionnalité de version d’évaluation](#implement-trial).
    * Si votre application offre un module complémentaire que les clients peuvent acheter, [créer une soumission de module complémentaire pour votre application dans le centre partenaires](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Si vous souhaitez exclure ou limiter certaines fonctionnalités dans une version d’évaluation de votre application, [configurer votre application en tant qu’une version d’évaluation gratuite dans partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

3. Avec votre projet ouvert dans Visual Studio, cliquez sur le **menu Projet**, pointez sur **Store**, puis cliquez sur **Associer l’application au Windows Store**. Suivez les instructions dans l’Assistant pour associer le projet d’application à l’application dans votre compte espace partenaires que vous souhaitez utiliser pour le test.
    > [!NOTE]
    > Si vous n’associez pas votre projet à une application du Windows Store, les méthodes [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) attribuent le code d’erreur 0x803F6107 à la propriété **ExtendedError** dans la valeur renvoyée. Cette valeur indique que le Windows Store ne connaît pas l’application.
4. Si ce n’est déjà fait, installez l’application à partir du Windows Store que vous avez spécifié à l’étape précédente, exécutez l’application une fois, puis fermez-la. Ceci garantit l’installation d’une licence valide de l’application sur votre appareil de développement.

5. Dans Visual Studio, exécutez ou déboguez votre projet. Votre code doit récupérer les données d’application et de module complémentaire, à partir de l’application Windows Store que vous avez associée à votre projet local. Si vous êtes invité à réinstaller l’application, suivez les instructions, puis exécutez ou déboguez votre projet.
    > [!NOTE]
    > Après avoir effectué ces étapes, vous pouvez continuer et mettre à jour le code de votre application, puis déboguer votre projet mis à jour sur votre ordinateur de développement sans soumettre de nouveaux packages d’application au Windows Store. Il vous suffit de télécharger la version Windows Store de votre application sur votre ordinateur de développement pour obtenir la licence locale qui sera utilisée pour les tests. Vous devez uniquement soumettre les nouveaux packages d’application au Windows Store après avoir terminé vos tests et proposer à vos clients les fonctionnalités liées aux achats in-app ou à la version d’évaluation de votre application.

Si votre application utilise l'espace de noms **Windows.ApplicationModel.Store**, vous pouvez utiliser la classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) dans votre application pour simuler les informations de licence pendant le test, avant de soumettre votre application au Windows Store. Pour plus d’informations, consultez [prise en main les classes CurrentApp et CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. Si vous utilisez l'espace de noms **Windows.Services.Store** pour implémenter les achats dans l'application ou les versions d’évaluation, vous devez publier votre application dans le Windows Store et télécharger l’application sur votre appareil de développement pour utiliser sa licence à des fins de test, comme indiqué ci-dessus.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Reçus des achats dans l’application

L’espace de noms **Windows.Services.Store** ne fournit aucune API vous permettant d’obtenir un reçu de transaction pour les achats effectués dans le code de votre application. Il s’agit d’une expérience différente de celles des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, qui peuvent elles [utiliser une API côté client pour récupérer un reçu de transaction](use-receipts-to-verify-product-purchases.md).

Si vous implémentez des achats dans l’application à l’aide de l’espace de noms **Windows.Services.Store** et que vous souhaitez confirmer l’achat par le client d’une app ou d’un module complémentaire, vous pouvez utiliser la [méthode de requête de produits](query-for-products.md) dans l’[API REST de collection du Microsoft Store](view-and-grant-products-from-a-service.md). Les données renvoyées avec cette méthode confirment si l’utilisateur considéré possède des droits pour un produit donné, et fournissent des données pour la transaction dans laquelle l’utilisateur a acquis le produit. L’API de collection du Microsoft Store utilise l’authentification Azure AD pour récupérer ces informations.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Utilisation de la classe StoreContext avec Desktop Bridge

Les applications de bureau qui utilisent [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) peuvent utiliser la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour implémenter des achats dans l’application et des versions d’évaluation. Toutefois, si vous possédez une application de bureau Win32 ou une application de bureau présentant un handle Windows associé à l’infrastructure de rendu (comme une application WPF), votre application doit configurer l’objet **StoreContext** pour identifier la fenêtre applicative du propriétaire pour les boîtes de dialogue modales affichées par l’objet.

De nombreux membres **StoreContext** (et des membres d’autres types associés accessibles via l’objet **StoreContext**) affichent une boîte de dialogue modale à l’utilisateur, pour les opérations associées au Windows Store telle que l’achat d’un produit. Si une application de bureau ne configure pas l’objet **StoreContext** afin de spécifier la fenêtre du propriétaire pour les boîtes de dialogue modales, cet objet renvoie des données inappropriées ou des erreurs.

Pour configurer un objet **StoreContext** dans une application de bureau qui utilise Desktop Bridge, procédez comme suit.

1. Pour permettre à votre application d’accéder à l’interface [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx), effectuez l’une des actions suivantes :

    * Si votre application est écrite dans un langage géré comme C# ou Visual Basic, déclarez l’interface **IInitializeWithWindow** dans le code de votre application avec l’attribut [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx), comme représenté dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction **using** pour l’espace de noms **System.Runtime.InteropServices**.

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

Chaque produit dans le Windows Store a au moins une *référence (SKU)*, et chaque référence (SKU) a au moins une *disponibilité*. Ces concepts sont retirés de la plupart des développeurs dans le centre de partenaires, et la plupart des développeurs ne définira jamais de références (SKU) ou des disponibilités pour leurs applications ou les modules complémentaires. Toutefois, comme le modèle d’objet des produits du Windows Store dans l’espace de noms **Windows.Services.Store** contient des références (SKU) et des disponibilités, la compréhension de ces concepts de base peut être utile pour certains scénarios.

| Objet |  Description  |
|---------|-------------------|
| Produit  |  Un *produit* désigne un type de produit disponible dans le Windows Store, notamment une application ou une extension. <p/><p/> Chaque produit dans le Windows Store est associé à un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Cette classe fournit des propriétés qui vous permettent d’accéder à des données, telles que l’ID Windows Store du produit, les images et vidéos de la liste du Windows Store, ainsi que des informations tarifaires. Elle fournit également des méthodes que vous pouvez utiliser pour acheter le produit. |
| SKU |  Une *référence (SKU)* est une version spécifique d’un produit avec sa propre description, son prix et d’autres détails uniques sur le produit. Chaque application ou module complémentaire a une référence (SKU) par défaut. Le seul cas où la plupart des développeurs ont plusieurs références (SKU) pour une application, c’est lorsqu’ils publient une version complète de leur application et une version d’évaluation. Dans le catalogue du Windows Store, chacune de ces versions a une référence (SKU) différente de la même application. <p/><p/> Certains éditeurs peuvent définir leurs propres références (SKU). Par exemple, un grand éditeur de jeux peut commercialiser un jeu avec une référence (SKU) qui affiche du sang vert sur les marchés qui n’autorisent pas le sang rouge et une autre référence (SKU) montrant du sang rouge sur tous les autres marchés. Autre possibilité, un éditeur qui vend des contenus vidéo numériques peut publier deux références (SKU) pour une vidéo : une pour la version haute définition et une autre pour la version en définition standard. <p/><p/> Chaque référence (SKU) dans le Windows Store est associé à un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx). Chaque [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) a une propriété [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) que vous pouvez utiliser pour accéder aux références (SKU) du produit. |
| Disponibilité  |  Une *disponibilité* est une version spécifique d’une référence (SKU) avec des informations de tarification qui lui sont propres. Chaque référence (SKU) a une disponibilité par défaut. Certains éditeurs peuvent définir leurs propres disponibilités pour proposer des prix différents pour une référence (SKU) donnée. <p/><p/> Chaque disponibilité dans le Windows Store est associée à un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx). Chaque [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) a une propriété [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) que vous pouvez utiliser pour accéder aux disponibilités de la référence (SKU). Pour la plupart des développeurs, chaque référence (SKU) a une disponibilité unique par défaut.  |

<span id="store_ids" />

### <a name="store-ids"></a>ID Windows Store

Chaque application, extension ou autre produit dans le Windows Store est associé à un **ID Store** (parfois également appelé *ID Store du produit*). Un grand nombre d’API requièrent l’ID Store pour effectuer une opération sur une application ou une extension.

L’ID Windows Store d’un produit dans le Windows Store est composé de 12 caractères alphanumériques, comme ```9NBLGGH4R315```. Il existe différentes méthodes pour obtenir l’ID Store d'un produit dans le Windows Store :

* Pour une application, vous pouvez obtenir l’ID de Store le [page identité des applications](../publish/view-app-identity-details.md) dans Partner Center.
* Pour un module complémentaire, vous pouvez obtenir l’ID de Store sur page de vue d’ensemble de l’add-on de partenaires.
* Pour un produit, vous pouvez également obtenir l’ID Store par programme à l’aide de la propriété [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) de l'objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représente le produit.

Pour les produits dotés de références (SKU) et de disponibilités, les références (SKU) et les disponibilités ont également leurs propres ID Windows Store avec différents formats.

| Objet |  Format de l’ID Windows Store  |
|---------|-------------------|
| SKU |  L’ID Store d'une référence (SKU) présente le format ```<product Store ID>/xxxx```, où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) du produit. Exemple : ```9NBLGGH4R315/000N```. Cet ID est retourné par la propriété [StoreID](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) d’un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) et il est parfois appelé *ID Windows Store de référence*. |
| Disponibilité  |  L’ID Store d'une disponibilité a le format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) du produit et ```yyyyyyyyyyyy``` est une chaîne alphanumérique de 12 caractères qui identifie une disponibilité de la référence (SKU). Exemple : ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Cet ID est retourné par la propriété [StoreID](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) d’un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) et il est parfois appelé *ID Windows Store de disponibilité*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Comment utiliser l’ID produit pour les extensions dans votre code

Si vous souhaitez proposer un module complémentaire à vos clients dans le contexte de votre application, vous devez [entrer un ID de produit unique](../publish/set-your-add-on-product-id.md#product-id) pour votre module complémentaire lorsque vous [créer votre soumission de module complémentaire](../publish/add-on-submissions.md) dans Partner Center. Vous pouvez utiliser cet ID produit pour faire référence à l'extension dans votre code, bien que les scénarios spécifiques dans lesquels vous pouvez utiliser l’ID produit dépendent de l’espace de noms que vous utilisez pour les achats in-app dans votre application.

> [!NOTE]
> L’ID de produit que vous entrez dans l’espace partenaires pour un module complémentaire est différente de celle du add-on [Store ID](#store-ids). L’ID de Store est généré par les partenaires.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.Services.Store

Si votre application utilise l'espace de noms **Windows.Services.Store**, vous pouvez utiliser l’ID produit pour identifier facilement le [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) qui représente votre extension ou le [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) qui représente la licence de votre extension. L'ID produit est exposé par les propriétés [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) et [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken).

> [!NOTE]
> Bien que l’ID produit soit un moyen utile pour identifier une extension dans votre code, la plupart des opérations dans l’espace de noms **Windows.Services.Store** utilisent l'[ID Store](#store-ids) d’une extension au lieu de l’ID produit. Par exemple, pour récupérer par programme une ou plusieurs extensions connues pour une application, passez les ID Store (plutôt que les ID produit) des extensions à la méthode [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). De même, pour signaler une extension consommable comme épuisée, passez l’ID Store de l’extension (plutôt que l’ID produit) à la méthode [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.ApplicationModel.Store

Si votre application utilise le **Windows.ApplicationModel.Store** espace de noms, vous devez utiliser l’ID de produit que vous attribuez à un module complémentaire de partenaires pour la plupart des opérations. Exemple :

* Utilisez l’ID produit pour identifier le [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) qui représente votre extension ou le [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) qui représente la licence de votre extension. L'ID produit est exposé par les propriétés [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) et [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId).

* Spécifiez l’ID produit lorsque vous achetez votre extension pour l’utilisateur à l’aide de la méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync). Pour plus d’informations, voir [Activer l’achat de produits dans l’application](enable-in-app-product-purchases.md).

* Spécifiez l’ID produit lorsque vous signalez votre extension consommable comme épuisée à l’aide de la méthode [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync). Pour plus d’informations, voir [Activer l’achat de produits dans l'application consommables](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Activer les modules complémentaires d’abonnement pour votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Codes d’erreur pour les opérations de Store](error-codes-for-store-operations.md)
* [Achats dans l’application et les versions d’évaluation à l’aide de l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
