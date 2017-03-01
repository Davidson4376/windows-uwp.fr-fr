---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour utiliser les extensions consommables."
title: "Activer les achats d’extensions consommables"
keywords: "windows 10, uwp, consommable, extensions, achats dans l’application, Windows.Services.Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d4cc4d526dfbfb2a120bc0a214b5b9287ec1acb3
ms.lasthandoff: 02/07/2017

---

# <a name="enable-consumable-add-on-purchases"></a>Activer les achats d’extensions consommables

Les applications qui ciblent Windows 10 version 1607 ou ultérieure peuvent utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour gérer la consommation par l’utilisateur d’extensions consommables (également appelées achats dans l’application) dans vos applications UWP. Utilisez les modules complémentaires consommables pour les éléments qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour obtenir des pouvoirs spécifiques.

>**Remarque**&nbsp;&nbsp;Cet article concerne les applications ciblant Windows 10 version 1607 ou ultérieure. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="overview-of-consumable-add-ons"></a>Présentation des modules complémentaires consommables

Les applications qui ciblent Windows 10 version 1607 ou ultérieure peuvent proposer deux types de modules complémentaires consommables qui se différencient par leur mode d’acquisition :

* **Consommable géré par le développeur**. Pour ce type de consommable, vous devez gérer le solde d’éléments de l’utilisateur, qui représente le module complémentaire, et signaler l’achat du module complémentaire comme épuisé dans le Windows Store une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé.

  Par exemple, si votre module complémentaire représente 100 pièces dans un jeu et que l’utilisateur en consomme 10, votre application ou votre service doit maintenir le nouveau solde restant de 90 pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100 pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100 pièces.

* **Consommable géré par le Windows Store**. Pour ce type de consommable, le Windows Store gère le solde d’éléments de l’utilisateur, que le module complémentaire représente. Lorsque l’utilisateur utilise des éléments, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. Votre application peut demander le solde actuel de l’utilisateur à tout moment. Une fois que l’utilisateur a consommé tous les éléments, l’utilisateur peut acheter le module complémentaire à nouveau.

  Par exemple, si votre module complémentaire représente une quantité initiale de 100 pièces dans un jeu et que l’utilisateur consomme 10 pièces, votre application signale au Store que 10 unités du module complémentaire ont été consommées, et le Windows Store met à jour le solde restant. Une fois que l’utilisateur a consommé les 100 pièces, l’utilisateur peut à nouveau acheter le module complémentaire de 100 pièces.

  >**Remarque**&nbsp;&nbsp;Les consommables gérés par le Windows Store sont disponibles à partir de Windows 10 version 1607. Il sera bientôt possible de créer un consommable géré par le Windows Store dans le tableau de bord du Centre de développement Windows.

Pour offrir un module complémentaire consommable à un utilisateur, suivez cette procédure générale :

1. Autorisez les utilisateurs à [acheter le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à partir de votre application.
3. Lorsque l’utilisateur consomme le module complémentaire (par exemple, en dépensant des pièces dans un jeu), [signalez le module comme consommé](enable-consumable-add-on-purchases.md#report_fulfilled).

À tout moment, vous pouvez également [consulter le solde restant](enable-consumable-add-on-purchases.md#get_balance) d’un consommable géré par le Windows Store.

## <a name="prerequisites"></a>Conditions préalables

Les conditions préalables de ces exemples sont les suivantes :
* Un projet Visual Studio d’application de plateforme Windows universelle (UWP) qui cible Windows 10 version 1607 ou ultérieure.
* Vous avez créé une application dans le tableau de bord du Centre de développement Windows avec des modules complémentaires consommables (également appelés produits dans l’application ou produits in-app), et celle-ci est publiée et disponible dans le Windows Store. Il peut s’agir d’une application que vous souhaitez vendre à des clients, ou d’une application de base qui présente la configuration minimale requise par le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) et que vous utilisez uniquement à des fins de test. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).

Le code de ces exemples respecte les présupposés suivants :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

Pour obtenir un exemple d’application complète, voir l’[exemple du Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Remarque**&nbsp;&nbsp;Si vous disposez d’une application de bureau qui utilise [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />
## <a name="report-a-consumable-add-on-as-fulfilled"></a>Signaler un composant additionnel consommable comme épuisé

Lorsque l’utilisateur [achète le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à votre application et qu’il le consomme, votre application doit le signaler comme épuisé en appelant la méthode [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Vous devez transmettre les informations suivantes à cette méthode :

* L’[ID Windows Store](in-app-purchases-and-trials.md#store_ids) du module complémentaire que vous souhaitez signaler comme consommé.
* Les unités du module complémentaire que vous souhaitez signaler comme épuisées.
  * Pour un consommable géré par le développeur, attribuez la valeur 1 au paramètre de *quantité*. Ceci signale au Windows Store que le consommable est épuisé et que le client peut l’acheter à nouveau. L’utilisateur ne peut pas acheter le consommable à nouveau tant que votre application n’a pas notifié au Windows Store qu’il a été épuisé.
  * Pour un consommable géré par le Windows Store, indiquez le nombre réel d’unités consommées. Le Windows Store met à jour le solde restant du consommable.
* L’ID de suivi de l’acquisition. Il s’agit d’un GUID fourni par le développeur, qui identifie la transaction spécifique à laquelle l’opération d’acquisition est associée, à des fins de suivi. Pour plus d’informations, consultez les remarques dans [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx).

Cet exemple montre comment signaler un consommable géré par le Windows Store comme épuisé.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />
## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtenir le solde restant d’un consommable géré par le Windows Store

Cet exemple montre comment utiliser la méthode [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour obtenir le solde restant d’un consommable géré par le Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

