---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations de licence de l’application active et de ses modules complémentaires.
title: Obtenir les informations de licence de votre application et des extensions
ms.author: mcleans
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, licences, applications, extensions, achats dans l’application, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 26a8ee69c291bd1b181cdc842175232a8310fd0d
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689375"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtenir les informations de licence des applications et des extensions

Cet article explique comment utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir des informations sur la licence de l’application actuelle et de ses extensions. Par exemple, vous pouvez utiliser ces informations pour déterminer si les licences de l’application ou de ses modules complémentaires sont actives ou s’il s’agit de licences d’évaluation.

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante:
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure.
* Vous avez [créé une soumission d'application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement Windows, et cette application est publiée dans le WindowsStore. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de licence pour une extension de l’application, vous devez également [créer l'extension dans le tableau de bord du Centre de développement](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Pour récupérer les informations de licence pour l’application actuelle, utilisez la méthode [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync). Il s’agit d’une méthode asynchrone qui renvoie un objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) fournissant les informations de licence pour l’app, notamment les propriétés indiquant si l’utilisateur dispose actuellement d’une licence valide pour utiliser l’app ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)) et si la licence est associée à une version d’évaluation ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Pour accéder aux licences des modules complémentaires durables de l’app pour laquelle l’utilisateur possède actuellement des droits d'utilisation, utilisez la propriété [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) de l'objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). Cette propriété renvoie une collection d’objets [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) qui représentent les licences des extensions.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Pour obtenir un exemple d’application complète, consultez [Exemple du MicrosoftStore](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
