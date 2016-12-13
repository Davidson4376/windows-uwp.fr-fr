---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations de licence de l’application active et de ses modules complémentaires."
title: Obtenir les informations de licence de votre application et des extensions
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 0482cc192eeff4d3633898b6fa677805c635c6e1

---

# <a name="get-license-info-for-apps-and-add-ons"></a>Obtenir les informations de licence des applications et des extensions

Les applications qui ciblent Windows&nbsp;10 version&nbsp;1607 ou ultérieure peuvent utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir les informations de licence de l’application actuelle et de ses modules complémentaires (également appelés produits dans l’application ou produits in-app). Par exemple, vous pouvez utiliser ces informations pour déterminer si les licences de l’application ou de ses modules complémentaires sont actives ou s’il s’agit de licences d’évaluation.

>**Remarque**&nbsp;&nbsp;Cet article concerne les applications ciblant Windows&nbsp;10 version&nbsp;1607 ou ultérieure. Si votre application cible une version antérieure de Windows&nbsp;10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante&nbsp;:
* Un projet Visual&nbsp;Studio d’application de plateforme Windows universelle (UWP) qui cible Windows&nbsp;10 version&nbsp;1607 ou ultérieure.
* Vous avez créé une application dans le tableau de bord du Centre de développement Windows, et celle-ci est publiée et disponible dans le Windows&nbsp;Store. Il peut s’agir d’une application que vous souhaitez vendre à des clients, ou d’une application de base qui présente la configuration minimale requise par le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) et que vous utilisez uniquement à des fins de test. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).

Le code de cet exemple se base sur les hypothèses suivantes&nbsp;:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

>**Remarque**&nbsp;&nbsp;Si vous disposez d’une application de bureau qui utilise [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Pour récupérer les informations de licence pour l’application actuelle, utilisez la méthode [GetAppLicenseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getapplicenseasync.aspx). Il s’agit d’une méthode asynchrone qui renvoie un objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) fournissant les informations de licence pour l’application, notamment les propriétés indiquant si l’utilisateur dispose d’une licence pour utiliser l’application ([IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.isactive.aspx)) et si la licence est associée à une version d’évaluation ([IsTrial](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.istrial.aspx)).

Pour récupérer les licences des extensions de l’application, utilisez la propriété [AddOnLicenses](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.addonlicenses.aspx) de l’objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Cette propriété renvoie une collection d’objets [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) qui représentent les licences des extensions de l’application. Pour déterminer si l’utilisateur dispose d’une licence pour utiliser un modèle complémentaire, utilisez la propriété [IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.isactive.aspx).

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Pour obtenir un exemple d’application complète, consultez [Exemple Windows&nbsp;Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


