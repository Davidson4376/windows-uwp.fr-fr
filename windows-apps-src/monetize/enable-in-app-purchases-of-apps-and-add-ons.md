---
author: Xansky
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour acheter une application ou l’une de ses extensions.
title: Activer les achats dans l’application et d’extensions dans l’application
keywords: windows10, uwp, extensions, achats dans l’application, Windows.Services.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7bf04632c4c99f2d58e3cd936e678af168750ff0
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5405303"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Activer les achats dans l’application et d’extensions dans l’application

Cet article montre comment utiliser des membres de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour solliciter l’achat de l’application actuelle ou de l’une de ses extensions pour l’utilisateur. Par exemple, si l’utilisateur possède actuellement une version d’évaluation de l’application, vous pouvez utiliser ce processus pour acheter une licence complète pour cet utilisateur. Vous pouvez aussi utiliser ce processus pour acheter une extension, comme un nouveau niveau de jeu pour l’utilisateur.

Pour solliciter l’achat d’une application ou d’une extension, l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) propose plusieurs méthodes différentes:
* Si vous connaissez l’[ID Store](in-app-purchases-and-trials.md#store_ids) de l’application ou de l’extension, vous pouvez utiliser la méthode [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Si vous avez déjà un objet [**StoreProduct**, **StoreSku** ou **StoreAvailability**](in-app-purchases-and-trials.md#products-skus) qui représente l’application ou l’extension, vous pouvez utiliser les méthodes **RequestPurchaseAsync** de ces objets. Pour obtenir des exemples des différentes façons de récupérer un **StoreProduct** dans votre code, voir [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md).

Chaque méthode présente une interface utilisateur d’achat standard à l’utilisateur et s’exécute de manière asynchrone une fois la transaction terminée. La méthode retourne un objet qui indique si la transaction a réussi.

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante:
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure.
* Vous avez [créé une soumission d'application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement Windows, et cette application est publiée dans le WindowsStore. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez activer les achats dans l’application pour une extension de l’application, vous devez également [créer l'extension dans le tableau de bord du Centre de développement](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Cet exemple montre comment utiliser la méthode [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour acheter une application ou une extension avec un [ID Windows Store](in-app-purchases-and-trials.md#store-ids) connu. Pour obtenir un exemple d’application complète, consultez [Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Vidéo

Regardez la vidéo suivante pour une vue d’ensemble de l’implémentation des achats in-app dans votre application.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des extensions](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
