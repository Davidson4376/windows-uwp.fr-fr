---
author: Xansky
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour utiliser les modules complémentaires consommables.
title: Activer les achats d’extensions consommables
keywords: windows10, uwp, consommable, extensions, achats dans l’application, Windows.Services.Store
ms.author: mhopkins
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e4687833b55f1456d298b552f5cce897f8b4eaa1
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656598"
---
# <a name="enable-consumable-add-on-purchases"></a>Activer les achats d’extensions consommables

Cet article montre comment utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour gérer la consommation par l’utilisateur d'extensions consommables dans vos applicationsUWP. Utilisez les modules complémentaires consommables pour les éléments qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter des améliorations spécifiques.

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Présentation des modules complémentaires consommables

Les applications peuvent proposer deuxtypes d'extensions consommables qui se différencient par leur mode d’acquisition:

* **Consommable géré par le développeur**. Pour ce type de consommable, vous devez gérer le solde d’éléments de l’utilisateur, qui représente le module complémentaire, et signaler l’achat du module complémentaire comme épuisé dans le WindowsStore une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé.

  Par exemple, si votre module complémentaire représente 100pièces dans un jeu et que l’utilisateur en consomme10, votre application ou votre service doit maintenir le nouveau solde restant de 90pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100pièces.

* **Consommable géré par le WindowsStore**. Pour ce type de consommable, le WindowsStore gère le solde d’éléments de l’utilisateur, que le module complémentaire représente. Lorsque l’utilisateur utilise des éléments, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. L’utilisateur peut acheter le module complémentaire autant de fois qu’il le souhaite (il est inutile de consommer les éléments d’abord). Votre application peut demander au Store le solde actuel de l’utilisateur à tout moment.

  Par exemple, si votre module complémentaire représente une quantité initiale de 100pièces dans un jeu et que l’utilisateur consomme 50pièces, votre application signale au Store que 50unités du module complémentaire ont été consommées, et le Store met à jour le solde restant. Si l’utilisateur rachète ensuite votre module complémentaire pour obtenir 100pièces de plus, il possède alors 150pièces au total.
    > [!NOTE]
    > Les consommables gérés dans le Store ont été introduits dans Windows10, version1607.

Pour offrir une extension consommable à un utilisateur, suivez cette procédure générale:

1. Autorisez les utilisateurs à [acheter le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à partir de votre application.
3. Lorsque l’utilisateur consomme le module complémentaire (par exemple, en dépensant des pièces dans un jeu), [signalez le module comme consommé](enable-consumable-add-on-purchases.md#report_fulfilled).

À tout moment, vous pouvez également [consulter le solde restant](enable-consumable-add-on-purchases.md#get_balance) d’un consommable géré par le WindowsStore.

## <a name="prerequisites"></a>Conditions préalables

Les conditions préalables de ces exemples sont les suivantes:
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure.
* Vous avez [créé une soumission d’application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans l’espace partenaires et que cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Vous avez [créé une extension consommable pour l’application](../publish/add-on-submissions.md) dans l’espace partenaires.

Le code de ces exemples respecte les présupposés suivants:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

Pour obtenir un exemple d’application complète, voir l’[exemple du Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Signaler un composant additionnel consommable comme épuisé

Lorsque l’utilisateur [achète le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à votre application et qu’il le consomme, votre application doit le signaler comme épuisé en appelant la méthode [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Vous devez transmettre les informations suivantes à cette méthode:

* L’[ID WindowsStore](in-app-purchases-and-trials.md#store-ids) du module complémentaire que vous souhaitez signaler comme consommé.
* Les unités du module complémentaire que vous souhaitez signaler comme épuisées.
  * Pour un consommable géré par le développeur, attribuez la valeur1 au paramètre de *quantité*. Ceci signale au WindowsStore que le consommable est épuisé et que le client peut l’acheter à nouveau. L’utilisateur ne peut pas acheter le consommable à nouveau tant que votre application n’a pas notifié au WindowsStore qu’il a été épuisé.
  * Pour un consommable géré par le WindowsStore, indiquez le nombre réel d’unités consommées. Le WindowsStore met à jour le solde restant du consommable.
* L’ID de suivi de l’acquisition. Il s’agit d’un GUID fourni par le développeur, qui identifie la transaction spécifique à laquelle l’opération d’acquisition est associée, à des fins de suivi. Pour plus d’informations, consultez les remarques dans [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

Cet exemple montre comment signaler un consommable géré par le Store comme épuisé.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtenir le solde restant d’un consommable géré par le WindowsStore

Cet exemple montre comment utiliser la méthode [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour obtenir le solde restant d’un consommable géré par le WindowsStore.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
