---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations de licence de l’application active et de ses modules complémentaires."
title: Obtenir les informations de licence de votre application et des extensions
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, licences, applications, extensions, achats dans l’application, Windows.Services.Store"
ms.openlocfilehash: dbd37ed219d47ad2c7631170c4299af708404243
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtenir les informations de licence des applications et des extensions

Les applications qui ciblent Windows10 version1607 ou ultérieure peuvent utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir les informations de licence de l’application actuelle et de ses extensions. Par exemple, vous pouvez utiliser ces informations pour déterminer si les licences de l’application ou de ses modules complémentaires sont actives ou s’il s’agit de licences d’évaluation.

> [!NOTE]
> Cet article concerne les applications ciblant Windows10 version1607 ou ultérieure. Si votre application cible une version antérieure de Windows10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante:
* Un projet VisualStudio d’application de plateforme Windows universelle (UWP) qui cible Windows10 version1607 ou ultérieure.
* Vous avez [créé une soumission d'application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement Windows, et cette application est publiée dans le WindowsStore. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de licence pour une extension de l’application, vous devez également [créer l'extension dans le tableau de bord du Centre de développement](../publish/add-on-submissions.md). 

Le code de cet exemple se base sur les hypothèses suivantes:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Pour récupérer les informations de licence pour l’application actuelle, utilisez la méthode [GetAppLicenseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getapplicenseasync.aspx). Il s’agit d’une méthode asynchrone qui renvoie un objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) fournissant les informations de licence pour l’application, notamment les propriétés indiquant si l’utilisateur dispose d’une licence pour utiliser l’application ([IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.isactive.aspx)) et si la licence est associée à une version d’évaluation ([IsTrial](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.istrial.aspx)).

Pour récupérer les licences des extensions de l’application, utilisez la propriété [AddOnLicenses](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.addonlicenses.aspx) de l’objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Cette propriété renvoie une collection d’objets [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) qui représentent les licences des extensions de l’application. Pour déterminer si l’utilisateur dispose d’une licence pour utiliser un modèle complémentaire, utilisez la propriété [IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.isactive.aspx).

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Pour obtenir un exemple d’application complète, consultez [Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
