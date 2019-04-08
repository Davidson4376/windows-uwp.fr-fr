---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour acheter une application ou l’une de ses extensions.
title: Activer les achats in-app d’applications et de modules complémentaires
keywords: windows 10, uwp, extensions, achats dans l’application, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a64a52005221c418ea82e8fffa9ecf94b6d1bef3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661724"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Activer les achats in-app d’applications et de modules complémentaires

Cet article montre comment utiliser des membres de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour solliciter l’achat de l’application actuelle ou de l’une de ses extensions pour l’utilisateur. Par exemple, si l’utilisateur possède actuellement une version d’évaluation de l’application, vous pouvez utiliser ce processus pour acheter une licence complète pour cet utilisateur. Vous pouvez aussi utiliser ce processus pour acheter une extension, comme un nouveau niveau de jeu pour l’utilisateur.

Pour solliciter l’achat d’une application ou d’une extension, l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) propose plusieurs méthodes différentes :
* Si vous connaissez l’[ID Windows Store](in-app-purchases-and-trials.md#store_ids) de l’application ou de l’extension, vous pouvez utiliser la méthode [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Si vous avez déjà un objet [**StoreProduct**, **StoreSku** ou **StoreAvailability**](in-app-purchases-and-trials.md#products-skus) qui représente l’application ou l’extension, vous pouvez utiliser les méthodes **RequestPurchaseAsync** de ces objets. Pour obtenir des exemples des différentes façons de récupérer un **StoreProduct** dans votre code, voir [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md).

Chaque méthode présente une interface utilisateur d’achat standard à l’utilisateur et s’exécute de manière asynchrone une fois la transaction terminée. La méthode retourne un objet qui indique si la transaction a réussi.

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows 10, version 1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante :
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure.
* Vous avez [créé une soumission de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans Centre partenaires et de cette application est publiée dans le Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez activer les achats dans l’application d’un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans partenaires](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Cet exemple montre comment utiliser la méthode [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour acheter une application ou une extension avec un [ID Windows Store](in-app-purchases-and-trials.md#store-ids) connu. Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Video

Regardez la vidéo suivante pour une vue d’ensemble de l’implémentation des achats in-app dans votre application.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Rubriques connexes

* [Achats dans l’application et essais](in-app-purchases-and-trials.md)
* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
