---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Découvrez comment activer les versions d’évaluation et achats in-app dans les applicationsUWP."
title: "Versions d’évaluation et achats in-app"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# Versions d’évaluation et achats in-app

Le SDK Windows fournit des API que vous pouvez utiliser pour ajouter des achats in-app et la fonctionnalité d’évaluation dans votre application de plateforme Windows universelle (UWP) afin de monétiser votre application et d’enrichir ses fonctionnalités. Ces API permettent également d’accéder aux informations de licence de votre application.

Pour ces scénarios, Windows10 offre deuxAPI différentes:

* Toutes les versions de Windows10 prennent en charge une API pour les achats in-app et les informations de licence dans l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx).

* Depuis Windows10 version1607, il existe une autre API pour les achats in-app et les informations de licence dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).  

Bien que les API dans ces espaces de noms servent les mêmes objectifs, elles sont conçues légèrement différemment et leur code n’est pas compatible. Si votre application cible Windows10 version1607 ou ultérieure, nous vous recommandons d’utiliser l’espace de noms **Windows.Services.Store**. Celui-ci prend en charge les types de module complémentaire les plus récents, comme les modules complémentaires consommables gérés par le Windows Store. Il est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par le Centre de développement Windows et le WindowsStore. L’espace de noms **Windows.Services.Store** est également plus performant.

Cet article présente les achats in-app des applicationsUWP et décrit l’espace de noms **Windows.Services.Store** disponible depuis Windows10 version1607. Pour plus d’informations sur l’utilisation des membres dans l’espace de noms **Windows.ApplicationModel.Store**, consultez [Achats in-app et versions d’évaluation utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).


## Vue d’ensemble des achats in-app dans les applicationsUWP

Cette section décrit les concepts de base décrivant le fonctionnement des achats in-app et des versions d’évaluation avec les applicationsUWP dans le WindowsStore. La plupart de ces concepts s’appliquent aux espaces de noms **Windows.Services.Store** et **Windows.ApplicationModel.Store**.

En général, chaque élément proposé dans le WindowsStore est appelé *produit*. La plupart des développeurs utilisent les types de produit suivants: *applications* et *modules complémentaires* (également appelés produits dans l’application ou produits in-app). Un module complémentaire fait référence à un produit ou une fonctionnalité que vous rendez disponibles à vos clients dans le cadre de votre application. Un module complémentaire peut représenter une fonctionnalité que votre application propose aux clients: par exemple, la devise à utiliser dans une application ou un jeu, les nouvelles cartes ou armes d’un jeu, la possibilité d’utiliser votre application sans publicité, ou le contenu numérique comme de la musique ou des vidéos pour les applications qui peuvent proposer ce type de contenu.

Chaque application ou module complémentaire a une licence qui indique si l’utilisateur est autorisé à l’utiliser. Si l’utilisateur est autorisé à utiliser l’application ou le module complémentaire en tant que version d’évaluation, la licence fournit également des informations supplémentaires sur la version d’évaluation.

Pour offrir un module complémentaire aux clients dans votre application, commencez par [définir les modules complémentaires de votre application dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Ensuite, écrivez du code dans votre application pour déterminer si l’utilisateur est autorisé à utiliser la fonctionnalité représentée par le module complémentaire, et proposez le module complémentaire à la vente à l’utilisateur comme un achat in-app, s’il ne possède pas encore la licence correspondante. Pour obtenir des exemples qui illustrent des tâches associées utilisant l’espace de noms **Windows.Services.Store** dans les applications qui ciblent Windows10 version1607 ou ultérieure, consultez les articles suivants:

* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour obtenir des exemples illustrant les tâches associées utilisant l’espace de noms **Windows.ApplicationModel.Store**, consultez [Achats in-app et versions d’évaluation utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

Tous les développeurs peuvent créer les types suivants de module complémentaire.

| Type de module complémentaire |  Description  |
|---------|-------------------|
| Durable  |  Module complémentaire conservé pendant la durée de vie que vous spécifiez dans le [tableau de bord du Centre de développement Windows](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties). <p/><p/>Par défaut, les modules complémentaires durables n’ont pas de date d’expiration et ne peuvent donc être achetés qu’une seule fois. Si vous spécifiez une durée pour le module complémentaire, l’utilisateur peut racheter le module complémentaire après l’expiration de ce dernier.  |
| Consommable géré par le développeur  |  Module complémentaire qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Ce type de module complémentaire est généralement utilisé pour les devises dans l’application. <p/><p/>Pour ce type de consommable, vous devez gérer le solde d’éléments de l’utilisateur, qui représente le module complémentaire, et signaler l’achat du module complémentaire comme épuisé dans le WindowsStore une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé. <p/><p/>Par exemple, si votre module complémentaire représente 100pièces dans un jeu et que l’utilisateur en consomme10, votre application ou votre service doit maintenir le nouveau solde restant de 90pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100pièces.    |
| Consommable géré par le WindowsStore  |  Module complémentaire qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Ce type de module complémentaire est généralement utilisé pour les devises dans l’application.<p/><p/>Pour ce type de consommable, le WindowsStore gère le solde d’éléments de l’utilisateur, que le module complémentaire représente. Lorsque l’utilisateur utilise des éléments, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. Votre application peut demander le solde actuel de l’utilisateur à tout moment. Une fois que l’utilisateur a consommé tous les éléments, l’utilisateur peut acheter le module complémentaire à nouveau.  <p/><p/> Par exemple, si votre module complémentaire représente une quantité initiale de 100pièces dans un jeu et que l’utilisateur consomme 10pièces, votre application signale au Store que 10unités du module complémentaire ont été consommées, et le Windows Store met à jour le solde restant. Une fois que l’utilisateur a consommé les 100pièces, l’utilisateur peut à nouveau acheter le module complémentaire de 100pièces. <p/><p/> **Les consommables gérés par le WindowsStore sont disponibles à partir de Windows10 version1607. Il sera bientôt possible de créer un consommable géré par le WindowsStore dans le tableau de bord du Centre de développement Windows.**  |

<span />

>**Remarque**&nbsp;&nbsp;D’autres types de module complémentaire, comme les modules complémentaires durables avec des packages (également appelés contenu téléchargeable ou DLC) ne sont disponibles que pour un groupe restreint de développeurs et ne sont pas traités dans cette documentation.

<span id="api_intro" />
## Présentation de l’espace de noms Windows.Services.Store

Le point d’entrée principal de l’espace de noms **Windows.Services.Store** est la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Cette classe fournit des méthodes que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses modules complémentaires disponibles, acheter une application ou un module complémentaire pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses modules complémentaires. Pour obtenir un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), effectuez l’une des opérations suivantes:

* Dans une application mono-utilisateur (c’est-à-dire une application qui ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée), utilisez la méthode [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour consulter et gérer les données du WindowsStore concernant l’utilisateur. La plupart des applications de plateforme Windows universelle (UWP) sont des applications mono-utilisateur.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Dans une application multi-utilisateur, utilisez la méthode [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour consulter et gérer les données du WindowsStore concernant un utilisateur connecté avec son compte Microsoft pendant qu’il utilise l’application. Pour plus d’informations sur les applications multi-utilisateur, consultez [Présentation des applications multi-utilisateur](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications). L’exemple suivant obtient un objet **StoreContext** pour le premier utilisateur disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

Lorsque vous avez un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), vous pouvez commencer à appeler des méthodes pour acheter une application ou un module complémentaire pour l’utilisateur actuel et d’autres tâches. Pour plus d’informations, consultez les articles suivants:

* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour un exemple complet montrant comment implémenter des versions d’évaluation et des achats in-app à l’aide de l’espace de noms **Windows.Services.Store**, consultez la section [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span />
### Modèle d’objet pour les produits, références et disponibilités

Chaque produit dans le WindowsStore a au moins une *référence (SKU)*, et chaque référence (SKU) a au moins une *disponibilité*. Ces concepts sont employés par la plupart des développeurs dans le tableau de bord du Centre de développement Windows. La plupart des développeurs ne définissent jamais les références (SKU) ou les disponibilités de leurs applications ou modules complémentaires. Toutefois, comme le modèle d’objet des produits du WindowsStore dans l’espace de noms **Windows.Services.Store** contient des références (SKU) et des disponibilités, la compréhension de ces concepts de base peut être utile.

| Type d’objet |  Description  |
|---------|-------------------|
| Produit  |  La classe [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) représente un type de produit disponible dans le WindowsStore, notamment une application ou un modèle complémentaire. Cette classe fournit des propriétés qui vous permettent d’accéder à des données, telles que l’ID WindowsStore du produit, les images et vidéos de la liste du WindowsStore, ainsi que des informations tarifaires. Elle fournit également des méthodes que vous pouvez utiliser pour acheter le produit. |
| Référence (SKU) |  La classe [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) représente la *référence (SKU)* d’un produit. Une référence (SKU) est une version spécifique d’un produit avec sa propre description, son prix et d’autres détails uniques sur le produit. Chaque application ou module complémentaire a une référence (SKU) par défaut. Le seul cas où la plupart des développeurs ont plusieurs références (SKU) pour une application, c’est lorsqu’ils publient une version complète de leur application et une version d’évaluation. Dans le catalogue du WindowsStore, chacune de ces versions a une référence (SKU) différente de la même application. <p/><p/> Certains éditeurs peuvent définir leurs propres références (SKU). Par exemple, un grand éditeur de jeux peut commercialiser un jeu avec une référence (SKU) qui affiche du sang vert sur les marchés qui n’autorisent pas le sang rouge et une autre référence (SKU) montrant du sang rouge sur tous les autres marchés. Autre possibilité, un éditeur qui vend des contenus vidéo numériques peut publier deuxréférences (SKU) pour une vidéo: une pour la version haute définition et une autre pour la version en définition standard. <p/><p/> Chaque produit a une propriété [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) que vous pouvez utiliser pour accéder aux références (SKU). |
| Disponibilité  |  La classe [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) représente la *disponibilité* d’une référence (SKU). Une disponibilité est une version spécifique d’une référence (SKU) avec des informations de tarification qui lui sont propres. Chaque référence (SKU) a une disponibilité par défaut. Certains éditeurs peuvent définir leurs propres disponibilités pour proposer des prix différents pour une référence (SKU) donnée. <p/><p/> Chaque référence (SKU) a une propriété [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) que vous pouvez utiliser pour accéder aux disponibilités. Pour la plupart des développeurs, chaque référence (SKU) a une disponibilité unique par défaut.  |

<span id="store_ids" />
### ID WindowsStore

Chaque application ou module complémentaire dans le WindowsStore a un **ID WindowsStore**. Un grand nombre d’API dans l’espace de noms **Windows.Services.Store** requièrent l’ID WindowsStore pour effectuer une opération sur une application ou un module complémentaire. Les produits, références (SKU) et disponibilités ont des ID WindowsStore de différents formats.

| Type d’objet |  Format de l’ID Windows Store  |
|---------|-------------------|
| Produit  |  L’ID WindowsStore d’un produit dans le WindowsStore est composé de 12caractères alphanumériques, comme ```9NBLGGH4R315```. Cet ID figure dans le tableau de bord du Centre de développement Windows de l’application ou du module complémentaire, et il est renvoyé par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) de l’objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Cet ID est parfois appelé *ID WindowsStore du produit*. |
| Référence (SKU) |  Pour une référence (SKU), l’ID WindowsStore présente le format ```<product Store ID>/xxxx```, où ```xxxx``` est une chaîne alphanumérique de 4caractères qui identifie une référence (SKU) du produit. Exemple: ```9NBLGGH4R315/000N```. Cet ID est renvoyé par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) d’un objet [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) et il est parfois appelé *ID Windows Store de référence*. |
| Disponibilité  |  Pour une disponibilité, l’ID WindowsStore a le format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, où ```xxxx``` est une chaîne alphanumérique de 4caractères qui identifie une référence (SKU) du produit et ```yyyyyyyyyyyy``` est une chaîne alphanumérique de 12caractères qui identifie une disponibilité de la référence (SKU). Exemple: ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Cet ID est renvoyé par la propriété [StoreID](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) d’un objet [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) et il est parfois appelé *ID WindowsStore de disponibilité*.  |

<span id="testing" />
### Test d’applications qui utilisent l’espace de noms Windows.Services.Store

L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. En revanche, vous devez publier une application dans le WindowsStore et télécharger cette application sur votre appareil de développement pour utiliser sa licence à des fins de test. Cette expérience est différente des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, car ces dernières peuvent utiliser la classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) pour simuler des informations de licence lors du test.

Si votre application utilise des API dans l’espace de noms **Windows.Services.Store** pour accéder aux informations de votre application et ses modules complémentaires, procédez comme suit pour tester votre code:

1. Si votre application est déjà publiée et disponible dans le WindowsStore et que vous voulez la mettre à jour pour qu’elle utilise les API dans l’espace de noms **Windows.Services.Store**, vous êtes prêt à commencer. Si vous souhaitez proposer des modules complémentaires pour l’application, veillez à [définir les modules complémentaires de votre application](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) dans le tableau de bord du Centre de développement.

  Si vous n’avez aucune application publiée et disponible dans le WindowsStore, créez une application de base qui répond aux conditions minimales requises par le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit), puis [envoyez cette application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) au tableau de bord du Centre de développement Windows. Si vous souhaitez proposer des modules complémentaires pour l’application, veillez à [définir les modules complémentaires de votre application](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Le cas échéant, vous pouvez [masquer l’application dans le WindowsStore](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) pendant le test.

2. Dans votre application, écrivez du code qui utilise l’une des méthodes [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms **Windows.Services.Store** pour effectuer des tâches comme [rendre les modules complémentaires disponibles pour l’application active](get-product-info-for-apps-and-add-ons.md), [acheter une application ou un module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md), ou [obtenir les informations de licence de votre application](get-license-info-for-apps-and-add-ons.md). Pour plus d’exemples, consultez la section Rubriques connexes ci-dessous.

3. Dans VisualStudio, cliquez sur le **menu Projet**, pointez sur **Store**, puis cliquez sur **Associer l’application au WindowsStore**. Suivez les instructions de l’Assistant pour associer le projet d’application à l’application dans votre compte du Centre de développement Windows à utiliser pour le test.

  >**Remarque**&nbsp;&nbsp;Si vous n’associez pas votre projet à une application du Windows Store, les méthodes [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) attribuent le code d’erreur0x803F6107 à la propriété **ExtendedError** dans la valeur renvoyée. Cette valeur indique que le WindowsStore ne connaît pas l’application.

4. Si ce n’est déjà fait, installez l’application à partir du WindowsStore que vous avez spécifié à l’étape précédente, exécutez l’application une fois, puis fermez-la. Ceci garantit l’installation d’une licence valide de l’application sur votre appareil de développement.

5. Dans VisualStudio, exécutez ou déboguez votre projet. Votre code doit récupérer les données d’application et de module complémentaire, à partir de l’application WindowsStore que vous avez associée à votre projet local. Si vous êtes invité à réinstaller l’application, suivez les instructions, puis exécutez ou déboguez votre projet.

## Rubriques connexes

* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


