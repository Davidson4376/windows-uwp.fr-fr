---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour acheter une application ou l’une de ses extensions."
title: "Activer les achats in-app d’applications et d’extensions"
keywords: "exemple de code d’une offre intégrée à l’application"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 05a93f3124324d7308f5494ad14a15bfd6a4e698

---

# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Activer les achats in-app d’applications et d’extensions

Les applications qui ciblent Windows 10 version 1607 ou ultérieure peuvent utiliser des membres de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour solliciter l’achat de l’application actuelle ou de l’une de ses extensions (également connues sous le nom produits in-app ou PIA) pour l’utilisateur. Par exemple, si l’utilisateur possède actuellement une version d’évaluation de l’application, vous pouvez utiliser ce processus pour acheter une licence complète pour cet utilisateur. Vous pouvez aussi utiliser ce processus pour acheter une extension, comme un nouveau niveau de jeu pour l’utilisateur.

Pour solliciter l’achat d’une application ou d’une extension, l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) propose plusieurs méthodes différentes :
* Si vous connaissez l’[ID Windows Store](in-app-purchases-and-trials.md#store_ids) de l’application ou de l’extension, vous pouvez utiliser la méthode [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).
* Si vous avez déjà un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx), [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) ou [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) qui représente l’application ou l’extension, vous pouvez utiliser les méthodes **RequestPurchaseAsync** de ces objets.

Chaque méthode présente une interface utilisateur d’achat standard à l’utilisateur et s’exécute de manière asynchrone une fois la transaction terminée. La méthode retourne un objet qui indique si la transaction a réussi.

>**Remarque**  Cet article concerne les applications ciblant Windows 10 version 1607 ou ultérieure. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante :
* Un projet Visual Studio d’application de plateforme Windows universelle (UWP) qui cible Windows 10 version 1607 ou ultérieure.
* Vous avez créé une application dans le tableau de bord du Centre de développement Windows, et celle-ci est publiée et disponible dans le Windows Store. Il peut s’agir d’une application que vous souhaitez vendre à des clients, ou d’une application de base qui présente la configuration minimale requise par le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) et que vous utilisez uniquement à des fins de test. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

>**Remarque**  Si vous disposez d’une application de bureau qui utilise [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Cet exemple montre comment utiliser la méthode [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour acheter une application ou une extension avec un [ID Windows Store](in-app-purchases-and-trials.md#store_ids) connu.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des extensions](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


