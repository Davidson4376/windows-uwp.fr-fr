---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Découvrez comment activer les achats dans l’application et les versions d’évaluation dans les applications UWP."
title: "Achats dans l’application et versions d’évaluation"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, achats dans l’application, extensions, versions d’évaluation, consommables, durables"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ade314f79edf73527f29e937f8eab987c902802f
ms.lasthandoff: 02/07/2017

---

# <a name="in-app-purchases-and-trials"></a>Achats dans l’application et versions d’évaluation

Le SDK Windows fournit des API que vous pouvez utiliser pour implémenter les fonctionnalités suivantes afin de générer davantage de revenus grâce à votre application de plateforme Windows universelle (UWP) :

* **Achats in-app**&nbsp;&nbsp;Que votre application soit gratuite ou non, vous pouvez vendre du contenu ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application.

* **Fonctionnalité de version d’évaluation**&nbsp;&nbsp;Si vous configurez votre application en tant que [version d’évaluation gratuite dans le tableau de bord du Centre de développement Windows](../publish/set-app-pricing-and-availability.md#free-trial), vous pouvez encourager vos clients à acheter la version complète en excluant ou en limitant certaines fonctionnalités durant la période d’évaluation. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article vous fournit une vue d’ensemble du fonctionnement des achats in-app et des versions d’évaluation dans les applications UWP.

<span id="choose-namespace" />
## <a name="choose-which-namespace-to-use"></a>Choisir l’espace de noms à utiliser

Il existe deux espaces de noms différents à utiliser pour ajouter des achats in-app et des fonctionnalités de version d’évaluation dans vos applications UWP, en fonction de la version de Windows 10 ciblée par vos applications. Bien que les API dans ces espaces de noms servent les mêmes objectifs, elles sont conçues légèrement différemment et leur code n’est pas compatible.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;À partir de Windows 10, version 1607, les applications peuvent utiliser l’API dans cet espace de noms pour implémenter les achats in-app et les versions d’évaluation. Nous vous recommandons d’utiliser les membres de cet espace de noms si votre application cible Windows 10, version 1607 ou une version postérieure. Celui-ci prend en charge les types d’extension les plus récents, comme les extensions consommables gérées par le Windows Store. Il est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par le Centre de développement Windows et le Windows Store. Pour plus d’informations sur cet espace de noms, consultez la section [Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store](#api_intro) de cet article.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;L’ensemble des versions de Windows 10 prennent également en charge une API plus ancienne pour les achats in-app et les versions d’évaluation dans cet espace de noms. Bien que l’ensemble des applications UWP exécutées dans Windows 10 peuvent utiliser cet espace de noms, ce dernier ne peut pas être mis à jour pour à l’avenir prendre en charge de nouveaux types de produits et de fonctionnalités dans le Centre de développement Windows et le Windows Store. Pour plus d’informations sur cet espace de noms, consultez la section [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

<span id="concepts" />
## <a name="basic-concepts"></a>Concepts de base

Cette section présente les concepts de base relatifs aux achats in-app et aux versions d’évaluation dans les applications UWP. La plupart de ces concepts s’appliquent aux espaces de noms **Windows.Services.Store** et **Windows.ApplicationModel.Store**, sauf indication contraire.

En général, chacun des éléments proposés dans le Windows Store est appelé *produit*. La plupart des développeurs utilisent les types de produit suivants : *applications* et *modules complémentaires* (également appelés produits dans l’application ou produits in-app).

Une extension désigne un produit ou une fonctionnalité que vous mettez à la disposition de vos clients dans le contexte de votre application : par exemple, la devise à utiliser dans une application ou un jeu, les nouvelles cartes ou armes d’un jeu, la possibilité d’utiliser votre application sans publicité, ou le contenu numérique comme de la musique ou des vidéos pour les applications qui peuvent proposer ce type de contenu. Chaque application ou module complémentaire a une licence qui indique si l’utilisateur est autorisé à l’utiliser. Si l’utilisateur est autorisé à utiliser l’application ou le module complémentaire en tant que version d’évaluation, la licence fournit également des informations supplémentaires sur la version d’évaluation.

Pour proposer un module complémentaire aux clients dans votre application, vous devez [définir le module complémentaire pour votre application dans le tableau de bord du Centre de développement](../publish/iap-submissions.md) afin de signaler cette information au Windows Store. Ensuite, votre application peut utiliser les API des espaces de noms **Windows.Services.Store** ou **Windows.ApplicationModel.Store** pour proposer le module complémentaire à la vente à l’utilisateur, en tant qu’achat in-app.

Les applications UWP peuvent offrir des types de modules complémentaires suivants.

| Type de module complémentaire |  Description  |
|---------|-------------------|
| Durable  |  Module complémentaire conservé pendant la durée de vie que vous spécifiez dans le [tableau de bord du Centre de développement Windows](../publish/enter-iap-properties.md). <p/><p/>Par défaut, les modules complémentaires durables n’ont pas de date d’expiration et ne peuvent donc être achetés qu’une seule fois. Si vous spécifiez une durée pour le module complémentaire, l’utilisateur peut racheter le module complémentaire après l’expiration de ce dernier. |
| Consommable géré par le développeur  |  Module complémentaire qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Ce type de module complémentaire est généralement utilisé pour les devises dans l’application. <p/><p/>Pour ce type de consommable, vous devez gérer le solde d’éléments de l’utilisateur, qui représente le module complémentaire, et signaler l’achat du module complémentaire comme épuisé dans le Windows Store une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé. <p/><p/>Par exemple, si votre module complémentaire représente 100 pièces dans un jeu et que l’utilisateur en consomme 10, votre application ou votre service doit maintenir le nouveau solde restant de 90 pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100 pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100 pièces.    |
| Consommable géré par le Windows Store  |  Module complémentaire qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Ce type de module complémentaire est généralement utilisé pour les devises dans l’application.<p/><p/>Pour ce type de consommable, le Windows Store gère le solde d’éléments de l’utilisateur, que le module complémentaire représente. Lorsque l’utilisateur utilise des éléments, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. Votre application peut demander le solde actuel de l’utilisateur à tout moment. Une fois que l’utilisateur a consommé tous les éléments, l’utilisateur peut acheter le module complémentaire à nouveau.  <p/><p/> Par exemple, si votre module complémentaire représente une quantité initiale de 100 pièces dans un jeu et que l’utilisateur consomme 10 pièces, votre application signale au Store que 10 unités du module complémentaire ont été consommées, et le Windows Store met à jour le solde restant. Une fois que l’utilisateur a consommé les 100 pièces, l’utilisateur peut à nouveau acheter le module complémentaire de 100 pièces. <p/><p/>**Remarque**&nbsp;&nbsp;Les consommables gérés par le Windows Store sont disponibles à partir de Windows 10 version 1607. Pour utiliser les consommables gérés par le Windows Store, votre application doit cibler Windows 10, version 1607 ou une version antérieure, et elle doit utiliser l’API de l’espace de noms **Windows.Services.Store** en lieu et place de l’espace de noms **Windows.ApplicationModel.Store**.  |

<span />

>**Remarque**&nbsp;&nbsp;D’autres types d’extension, comme les extensions durables avec des packages (également appelés contenu téléchargeable ou DLC) ne sont disponibles que pour un groupe restreint de développeurs et ne sont pas traités dans cette documentation.

<span id="api_intro" />
## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store

Les parties suivantes de cet article décrivent l’implémentation des achats dans l’application et des versions d’évaluation utilisant l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Cet espace de noms est disponible uniquement pour les applications ciblant Windows 10, version 1607 ou une version antérieure, et nous recommandons l’utilisation de cet espace de noms en lieu et place de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx), dans la mesure du possible.

Pour plus d’informations sur l’espace de noms **Windows.ApplicationModel.Store**, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

### <a name="get-started-with-the-storecontext-class"></a>Démarrer avec la classe StoreContext

Le point d’entrée principal de l’espace de noms **Windows.Services.Store** est la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Cette classe fournit des méthodes que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses modules complémentaires disponibles, acheter une application ou un module complémentaire pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses modules complémentaires. Pour obtenir un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), effectuez l’une des opérations suivantes :

* Dans une application mono-utilisateur (c’est-à-dire une application qui ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée), utilisez la méthode statique [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du Windows Store concernant l’utilisateur. La plupart des applications de plateforme Windows universelle (UWP) sont des applications mono-utilisateur.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Dans une [application multi-utilisateur](../xbox-apps/multi-user-applications.md), utilisez la méthode statique [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données du Windows Store concernant un utilisateur connecté avec son compte Microsoft pendant qu’il utilise l’application. L’exemple suivant obtient un objet **StoreContext** pour le premier utilisateur disponible.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

>**Remarque**&nbsp;&nbsp;Les applications de bureau Windows qui utilisent [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) doivent exécuter des tâches supplémentaires de configuration de l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) avant de pouvoir l’utiliser. Pour plus d’informations, consultez [cette section](#desktop).

Une fois que vous possédez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), vous pouvez commencer à appeler des méthodes de cet objet pour obtenir des informations produit du Windows Store pour l’application actuelle et ses extensions, récupérer des informations de licence de l’application actuelle et de ses extensions, acheter une application ou une extension pour l’utilisateur actuel, et effectuer d’autres tâches. Pour plus d’informations sur les tâches courantes que vous pouvez exécuter à l’aide de cet objet, consultez les articles suivants :

* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour consulter un exemple d’application illustrant l’utilisation de l’objet **StoreContext** et d’autres types dans l’espace de noms **Windows.Services.Store**, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />
### <a name="implement-in-app-purchases"></a>Implémenter des achats dans l’application

Pour proposer un achat dans l’application aux clients de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. Si votre application propose des modules complémentaires pouvant être achetés par vos clients, [créez des soumissions de module complémentaire pour votre application dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit pour votre application ou un module complémentaire offert par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence est active](get-license-info-for-apps-and-add-ons.md) (c’est-à-dire, si l’utilisateur dispose d’une licence lui permettant d’utiliser l’application ou le module complémentaire). Si la licence n’est pas active, affichez une interface utilisateur qui propose à l’utilisateur l’application ou le module complémentaire à la vente en tant qu’achat in-app.

3. Si l’utilisateur choisit d’acheter votre application ou extension, utilisez la méthode d’achat appropriée du produit :

  * Si l’utilisateur achète votre application ou une extension durable, suivez les instructions de la rubrique [Activer les achats in-app d’applications et d’extensions](enable-in-app-purchases-of-apps-and-add-ons.md).

  * Si l’utilisateur achète une extension consommable, suivez les instructions de la rubrique [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="implement-trial" />
### <a name="implement-trial-functionality"></a>Implémenter les fonctionnalités des versions d’évaluation

Pour exclure ou limiter les fonctionnalités d’une version d’évaluation de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. [Configurez votre application en tant que version d’évaluation gratuite dans le tableau de bord du Centre de développement Windows](../publish/set-app-pricing-and-availability.md#free-trial).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit de votre application et d’un module complémentaire pris en charge par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence associée à l’application est une licence d’évaluation](get-license-info-for-apps-and-add-ons.md).

3. Excluez ou limitez certaines fonctionnalités de votre application s’il s’agit d’une version d’évaluation, puis activez les fonctionnalités quand l’utilisateur acquiert une licence complète. Pour obtenir plus d’informations et un exemple de code, voir [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="testing" />
### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation

L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. En revanche, vous devez publier une application dans le Windows Store et télécharger cette application sur votre appareil de développement pour utiliser sa licence à des fins de test. Cette expérience est différente des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, car ces dernières peuvent utiliser la classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) pour simuler des informations de licence lors du test.

Si votre application utilise des API dans l’espace de noms **Windows.Services.Store** pour accéder aux informations de votre application et ses modules complémentaires, procédez comme suit pour tester votre code :

1. Si votre application n’est pas encore publiée ni rendue disponible dans le Windows Store, assurez-vous qu’elle respecte les exigences minimales du [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit), [soumettez-la](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement Windows, et vérifiez qu’elle satisfait au processus de certification afin d’être répertoriée dans le Windows Store. Vous pouvez [masquer votre application dans le Windows Store](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) afin de la rendre indisponible aux clients durant le test.

2. Ensuite, assurez-vous d’exécuter les actions suivantes :

  * Écrivez du code dans votre application qui utilise la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) et d’autres types associés dans l’espace de noms **Windows.Services.Store** afin d’implémenter les [achats dans l’application](#implement-iap) ou une [fonctionnalité de version d’évaluation](#implement-trial).

  * Si votre application propose une extension pouvant être achetée par vos clients, [créez une soumission d’extension pour votre application dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

  * Si vous souhaitez exclure ou limiter certaines fonctionnalités dans une version d’évaluation de votre application, [configurez votre application en tant que version d’évaluation gratuite dans le tableau de bord du Centre de développement Windows](../publish/set-app-pricing-and-availability.md#free-trial).

3. Avec votre projet ouvert dans Visual Studio, cliquez sur le **menu Projet**, pointez sur **Store**, puis cliquez sur **Associer l’application au Windows Store**. Suivez les instructions de l’Assistant pour associer le projet d’application à l’application dans votre compte du Centre de développement Windows à utiliser pour le test.

  >**Remarque**&nbsp;&nbsp;Si vous n’associez pas votre projet à une application du Windows Store, les méthodes [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) attribuent le code d’erreur 0x803F6107 à la propriété **ExtendedError** dans la valeur renvoyée. Cette valeur indique que le Windows Store ne connaît pas l’application.

4. Si ce n’est déjà fait, installez l’application à partir du Windows Store que vous avez spécifié à l’étape précédente, exécutez l’application une fois, puis fermez-la. Ceci garantit l’installation d’une licence valide de l’application sur votre appareil de développement.

5. Dans Visual Studio, exécutez ou déboguez votre projet. Votre code doit récupérer les données d’application et de module complémentaire, à partir de l’application Windows Store que vous avez associée à votre projet local. Si vous êtes invité à réinstaller l’application, suivez les instructions, puis exécutez ou déboguez votre projet.

>**Remarque**&nbsp;&nbsp;Après avoir effectué les étapes 1 à 5, vous pouvez continuer et mettre à jour le code de votre application, puis déboguer votre projet mis à jour sur votre ordinateur de développement sans soumettre de nouveaux packages d’application au Windows Store. Il vous suffit de télécharger la version Windows Store de votre application sur votre ordinateur de développement pour obtenir la licence locale qui sera utilisée pour les tests. Vous devez uniquement soumettre les nouveaux packages d’application au Windows Store après avoir terminé vos tests et proposer à vos clients les fonctionnalités liées aux achats in-app ou à la version d’évaluation de votre application.

<span id="receipts" />
### <a name="receipts-for-in-app-purchases"></a>Reçus des achats dans l’application

L’espace de noms **Windows.Services.Store** ne fournit aucune API vous permettant d’obtenir un reçu de transaction pour les achats effectués dans le code de votre application. Il s’agit d’une expérience différente de celles des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, qui peuvent elles [utiliser une API côté client pour récupérer un reçu de transaction](use-receipts-to-verify-product-purchases.md).

Si vous implémentez des achats dans l’application à l’aide de l’espace de noms **Windows.Services.Store** et que vous souhaitez confirmer l’achat par le client d’une application ou d’un module complémentaire, vous pouvez utiliser la [méthode de requête de produits](query-for-products.md) dans l’[API REST de collection du Windows Store](view-and-grant-products-from-a-service.md). Les données renvoyées avec cette méthode confirment si l’utilisateur considéré possède des droits pour un produit donné, et fournissent des données pour la transaction dans laquelle l’utilisateur a acquis le produit. L’API de collection du Windows Store utilise l’authentification Azure AD pour récupérer ces informations.

<span id="desktop" />
### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Utilisation de la classe StoreContext avec Desktop Bridge

Les applications de bureau qui utilisent [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) peuvent utiliser la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour implémenter des achats dans l’application et des versions d’évaluation. Toutefois, si vous possédez une application de bureau Win32 ou une application de bureau présentant un handle Windows associé à l’infrastructure de rendu (comme une application WPF), votre application doit configurer l’objet **StoreContext** pour identifier la fenêtre applicative du propriétaire pour les boîtes de dialogue modales affichées par l’objet.

De nombreux membres **StoreContext** (et des membres d’autres types associés accessibles via l’objet **StoreContext**) affichent une boîte de dialogue modale à l’utilisateur, pour les opérations associées au Windows Store telle que l’achat d’un produit. Si une application de bureau ne configure pas l’objet **StoreContext** afin de spécifier la fenêtre du propriétaire pour les boîtes de dialogue modales, cet objet renvoie des données inappropriées ou des erreurs.

Pour configurer un objet **StoreContext** dans une application de bureau qui utilise Desktop Bridge, procédez comme suit.

  1. Pour permettre à votre application d’accéder à l’interface [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx), effectuez l’une des actions suivantes :

    * Si votre application est écrite dans un langage géré comme C# ou Visual Basic, déclarez l’interface **IInitializeWithWindow** dans le code de votre application avec l’attribut [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx), comme représenté dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction **using** pour l’espace de noms **System.Runtime.InteropServices**.

      > [!div class="tabbedCodeSnippets"]
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

  2. Récupérez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en utilisant la méthode [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) (ou la méthode [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) si votre application est une [application multi-utilisateur](../xbox-apps/multi-user-applications.md)) comme décrit plus haut dans cet article, puis convertissez cet objet en objet [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx), puis transmettez le handle de la fenêtre que vous souhaitez configurer comme propriétaire pour toutes les boîtes de dialogue modales affichées par les méthodes **StoreContext**. L’exemple suivant en C# vous explique comment transmettre le handle de la fenêtre principale de votre application à la méthode.

    > [!div class="tabbedCodeSnippets"]
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />
### <a name="products-skus-and-availabilities"></a>Produits, références et disponibilités

Chaque produit dans le Windows Store a au moins une *référence (SKU)*, et chaque référence (SKU) a au moins une *disponibilité*. Ces concepts sont employés par la plupart des développeurs dans le tableau de bord du Centre de développement Windows. La plupart des développeurs ne définissent jamais les références (SKU) ou les disponibilités de leurs applications ou modules complémentaires. Toutefois, comme le modèle d’objet des produits du Windows Store dans l’espace de noms **Windows.Services.Store** contient des références (SKU) et des disponibilités, la compréhension de ces concepts de base peut être utile.

| Type d’objet |  Description  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  Cette classe représente un type de produit disponible dans le Windows Store, notamment une application ou un modèle complémentaire. Cette classe fournit des propriétés qui vous permettent d’accéder à des données, telles que l’ID Windows Store du produit, les images et vidéos de la liste du Windows Store, ainsi que des informations tarifaires. Elle fournit également des méthodes que vous pouvez utiliser pour acheter le produit. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  Cette classe représente une *référence* de produit. Une référence (SKU) est une version spécifique d’un produit avec sa propre description, son prix et d’autres détails uniques sur le produit. Chaque application ou module complémentaire a une référence (SKU) par défaut. Le seul cas où la plupart des développeurs ont plusieurs références (SKU) pour une application, c’est lorsqu’ils publient une version complète de leur application et une version d’évaluation. Dans le catalogue du Windows Store, chacune de ces versions a une référence (SKU) différente de la même application. <p/><p/> Certains éditeurs peuvent définir leurs propres références (SKU). Par exemple, un grand éditeur de jeux peut commercialiser un jeu avec une référence (SKU) qui affiche du sang vert sur les marchés qui n’autorisent pas le sang rouge et une autre référence (SKU) montrant du sang rouge sur tous les autres marchés. Autre possibilité, un éditeur qui vend des contenus vidéo numériques peut publier deux références (SKU) pour une vidéo : une pour la version haute définition et une autre pour la version en définition standard. <p/><p/> Chaque produit a une propriété [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) que vous pouvez utiliser pour accéder aux références (SKU). |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  Cette classe représente une *disponibilité* de référence. Une disponibilité est une version spécifique d’une référence (SKU) avec des informations de tarification qui lui sont propres. Chaque référence (SKU) a une disponibilité par défaut. Certains éditeurs peuvent définir leurs propres disponibilités pour proposer des prix différents pour une référence (SKU) donnée. <p/><p/> Chaque référence (SKU) a une propriété [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) que vous pouvez utiliser pour accéder aux disponibilités. Pour la plupart des développeurs, chaque référence (SKU) a une disponibilité unique par défaut.  |

<span id="store_ids" />
### <a name="store-ids"></a>ID Windows Store

Chaque application ou module complémentaire dans le Windows Store a un **ID Windows Store**. Un grand nombre d’API dans l’espace de noms **Windows.Services.Store** requièrent l’ID Windows Store pour effectuer une opération sur une application ou un module complémentaire. Les produits, références (SKU) et disponibilités ont des ID Windows Store de différents formats.

| Type d’objet |  Format de l’ID Windows Store  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  L’ID Windows Store d’un produit dans le Windows Store est composé de 12 caractères alphanumériques, comme ```9NBLGGH4R315```. Cet ID figure dans le tableau de bord du Centre de développement Windows de l’application ou du module complémentaire, et il est renvoyé par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) de l’objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Cet ID est parfois appelé *ID Windows Store du produit*. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  Pour une référence (SKU), l’ID Windows Store présente le format ```<product Store ID>/xxxx```, où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) du produit. Exemple : ```9NBLGGH4R315/000N```. Cet ID est retourné par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) d’un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) et il est parfois appelé *ID Windows Store de référence*. |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  Pour une disponibilité, l’ID Windows Store a le format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) du produit et ```yyyyyyyyyyyy``` est une chaîne alphanumérique de 12 caractères qui identifie une disponibilité de la référence (SKU). Exemple : ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Cet ID est retourné par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) d’un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) et il est parfois appelé *ID Windows Store de disponibilité*.  |

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)

