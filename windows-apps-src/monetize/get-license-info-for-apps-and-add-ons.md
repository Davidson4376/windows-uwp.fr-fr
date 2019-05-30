---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations de licence de l’application active et de ses modules complémentaires.
title: Obtenir les informations de licence de votre application et des extensions
ms.date: 12/04/2017
ms.topic: article
keywords: windows 10, uwp, licences, applications, extensions, achats dans l’application, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: c8bef40cb34412fbb92585d2ccd25682c72ffe5c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371634"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtenir les informations de licence des applications et des modules complémentaires

Cet article explique comment utiliser les méthodes de la classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) pour obtenir des informations sur la licence de l’application actuelle et de ses extensions. Par exemple, vous pouvez utiliser ces informations pour déterminer si les licences de l’application ou de ses modules complémentaires sont actives ou s’il s’agit de licences d’évaluation.

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows 10, version 1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Prérequis

La configuration requise pour cet exemple est la suivante :
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure.
* Vous avez [créé une soumission de l’application](https://docs.microsoft.com/windows/uwp/publish/app-submissions) dans Centre partenaires et de cette application est publiée dans le Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de licence pour un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans partenaires](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Pour récupérer les informations de licence pour l’application actuelle, utilisez la méthode [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync). Il s’agit d’une méthode asynchrone qui renvoie un objet [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense) fournissant les informations de licence pour l’app, notamment les propriétés indiquant si l’utilisateur dispose actuellement d’une licence valide pour utiliser l’app ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)) et si la licence est associée à une version d’évaluation ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Pour accéder aux licences des modules complémentaires durables de l’app pour laquelle l’utilisateur possède actuellement des droits d'utilisation, utilisez la propriété [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) de l'objet [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense). Cette propriété renvoie une collection d’objets [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) qui représentent les licences des extensions.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Achats dans l’application et essais](in-app-purchases-and-trials.md)
* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
