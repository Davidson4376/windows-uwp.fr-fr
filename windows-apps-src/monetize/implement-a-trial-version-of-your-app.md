---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour implémenter une version d’évaluation de votre application.
title: Implémenter une version d’évaluation de votre application
keywords: windows 10, uwp, évaluation, achats dans l’application, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 796266565965a62d3f168b48893d62e1cdd7df44
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646164"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implémenter une version d’évaluation de votre application

Si vous [configurer votre application en tant qu’une version d’évaluation gratuite dans partenaires](../publish/set-app-pricing-and-availability.md#free-trial) afin que les clients peuvent utiliser votre application gratuitement pendant une période d’essai, vous pouvez inciter vos clients à mettre à niveau vers la version complète de votre application à exclure ou de limiter certaines fonctionnalités pendant la période d’évaluation. Choisissez les fonctionnalités à limiter avant de commencer à coder, puis faites en sorte que votre application ne les rende disponibles qu’à l’achat de la licence complète. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article montre comment utiliser des membres de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour déterminer si l’utilisateur possède une licence de version d’évaluation pour votre application et être averti si l’état de cette licence change pendant l’exécution de votre application. 

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows 10, version 1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Recommandations en matière d’implémentation d’une version d’évaluation

L’état de licence actuel de votre application est stocké dans les propriétés de la classe [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx). D’une manière générale, vous placez les fonctions qui dépendent de l’état de la licence dans un bloc conditionnel, comme nous le décrivons à l’étape suivante. Lorsque vous développez ces fonctionnalités, assurez-vous de pouvoir les implémenter de telle manière qu’elles fonctionnent avec tous les états de la licence.

Décidez également comment vous voulez gérer les modifications apportées à la licence de l’application lorsque celle-ci est en cours d’exécution. La version d’évaluation de votre application peut être entièrement fonctionnelle, mais comporter des bandeaux publicitaires intégrés à l’application, contrairement à la version payante. La version d’évaluation de votre application peut également être privée de certaines fonctionnalités ou afficher régulièrement des messages demandant à l’utilisateur de l’acheter.

Pensez au type d’application que vous développez et demandez-vous quelle est la meilleure stratégie d’évaluation ou d’expiration. Pour la version d’évaluation d’un jeu, une bonne stratégie est de limiter le contenu du jeu auquel un utilisateur peut avoir accès. Dans le cas de la version d’évaluation d’un utilitaire, vous pouvez envisager de définir une date d’expiration ou de limiter les fonctionnalités qu’un acheteur potentiel peut utiliser.

Pour la plupart des applications qui ne sont pas des jeux, la définition d’une date d’expiration est une bonne méthode, car cela permet aux utilisateurs d’avoir une idée correcte de l’application complète. Voici quelques scénarios d’expiration classiques et comment les gérer.

-   **Licence d’essai expire pendant que l’application est en cours d’exécution**

    Si la version d’évaluation expire tandis que votre application est en cours d’exécution, cette dernière peut :

    -   Ne rien faire.
    -   Afficher un message à l’attention de votre client.
    -   Fermer.
    -   Inviter votre client à acheter l’application.

    La pratique recommandée est d’afficher un message invitant l’utilisateur à acheter l’application, et si c’est le cas, de poursuivre l’exécution avec toutes les fonctionnalités activées. Sinon, fermez l’application ou redemandez régulièrement à l’utilisateur s’il souhaite acheter l’application.

-   **Licence d’essai expire avant que l’application est lancée.**

    Si la version d’évaluation expire avant que l’utilisateur ne lance l’application, cette dernière ne démarre pas. Au lieu de cela, une boîte de dialogue s’affiche pour donner à l’utilisateur la possibilité d’acheter votre application dans le Windows Store.

-   **Le client achète l’application pendant son exécution**

    Si le client achète votre application tandis que celle-ci est en cours d’exécution, voici quelques actions possibles.

    -   Ne rien faire et continuer en mode d’évaluation jusqu’au redémarrage de l’application.
    -   Les remercier de leur achat ou afficher un message.
    -   Activer de manière silencieuse les fonctionnalités qui sont disponibles avec une licence complète (ou désactiver les notifications réservées à la version d’évaluation).

Prenez soin d’expliquer à vos clients comment votre application se comportera pendant et après la période d’évaluation gratuite afin qu’ils ne soient pas surpris. Pour plus d’informations sur la description de votre application, voir [Créer des descriptions d’application](https://msdn.microsoft.com/library/windows/apps/mt148529).

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour cet exemple est la suivante :
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure.
* Vous avez créé une application dans les partenaires qui est configuré comme un [version d’évaluation gratuite](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) avec aucune limite de temps et de cette application est publiée dans le Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans cet exemple pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Lors du lancement de votre application, obtenez l’objet [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) de votre application et gérez l’événement [OfflineLicensesChanged](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) pour recevoir des notifications de modification de la licence pendant l’exécution de l’application. Par exemple, la licence de l’application peut changer si la période d’évaluation arrive à expiration ou si le client achète l’application par le biais du Windows Store. Quand la licence change, obtenez la nouvelle et activez ou désactivez une fonctionnalité de votre application en conséquence.

À ce stade, si un utilisateur a acheté l’application, il est de bonne pratique de lui indiquer que l’état de sa licence a été modifié. Demandez à l’utilisateur de redémarrer l’application si vous avez conçu votre application ainsi. Rendez cependant cette transition aussi simple que possible.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et les achats dans l’application](in-app-purchases-and-trials.md)
* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Exemple de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
