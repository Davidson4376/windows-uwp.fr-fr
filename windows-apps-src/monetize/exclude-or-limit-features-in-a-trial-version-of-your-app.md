---
Description: If you enable customers to use your app for free during a trial period, you can entice your customers to upgrade to the full version of your app by excluding or limiting some features during the trial period.
title: Exclure ou limiter des fonctionnalités de la version d’évaluation
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: windows10, uwp, essai, achat in-app, PIA, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 36d7ada6567db95609203f8f163b78631e141b4f
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736319"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>Exclure ou limiter des fonctionnalités de la version d’évaluation

Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. Choisissez les fonctionnalités à limiter avant de commencer à coder, puis faites en sorte que votre application ne les rende disponibles qu’à l’achat de la licence complète. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

> [!IMPORTANT]
> Cet article explique comment utiliser des membres de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour activer la fonctionnalité de version d'essai. Cet espace de noms n’est plus mis à jour avec de nouvelles fonctionnalités et nous vous recommandons d’utiliser l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) à la place. L’espace de noms **Windows.Services.Store** prend en charge les types de module complémentaire plus récents, tels que les extensions consommables gérés par le Windows Store et les abonnements et est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par l’espace partenaires et le Windows Store. L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations sur la mise en place de la fonctionnalité de version d'évaluation à l'aide de l'espace de noms **Windows.Services.Store**, consultez [cet article](implement-a-trial-version-of-your-app.md).

## <a name="prerequisites"></a>Prérequis

Application Windows dans laquelle ajouter des fonctionnalités que les clients peuvent acheter

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>Étape1: Sélection des fonctionnalités à activer ou désactiver durant la période d’évaluation

L’état actuel de la licence de votre application est stocké dans les propriétés de la classe [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157). En général, vous placez les fonctions qui dépendent de l’état de la licence dans un bloc conditionnel, comme expliqué à l’étape suivante. Lorsque vous développez ces fonctionnalités, assurez-vous de pouvoir les implémenter de telle manière qu’elles fonctionnent avec tous les états de la licence.

Décidez également comment vous voulez gérer les modifications apportées à la licence de l’application lorsque celle-ci est en cours d’exécution. La version d’évaluation de votre application peut être entièrement fonctionnelle, mais comporter des bandeaux publicitaires intégrés à l’application, contrairement à la version payante. La version d’évaluation de votre application peut également être privée de certaines fonctionnalités ou afficher régulièrement des messages demandant à l’utilisateur de l’acheter.

Pensez au type d’application que vous développez et demandez-vous quelle est la meilleure stratégie d’évaluation ou d’expiration. Pour la version d’évaluation d’un jeu, une bonne stratégie est de limiter le contenu du jeu auquel un utilisateur peut avoir accès. Dans le cas de la version d’évaluation d’un utilitaire, vous pouvez envisager de définir une date d’expiration ou de limiter les fonctionnalités qu’un acheteur potentiel peut utiliser.

Pour la plupart des applications qui ne sont pas des jeux, la définition d’une date d’expiration est une bonne méthode, car cela permet aux utilisateurs d’avoir une idée correcte de l’application complète. Voici quelques scénarios d’expiration classiques et comment les gérer.

-   **La licence d’évaluation expire tandis que l’application est en cours d’exécution.**

    Si la version d’évaluation expire tandis que votre application est en cours d’exécution, cette dernière peut :

    -   Ne rien faire.
    -   Afficher un message à l’attention de votre client.
    -   Se fermer.
    -   Inviter votre client à acheter l’application.

    La pratique recommandée est d’afficher un message invitant l’utilisateur à acheter l’application, et si c’est le cas, de poursuivre l’exécution avec toutes les fonctionnalités activées. Sinon, fermez l’application ou redemandez régulièrement à l’utilisateur s’il souhaite acheter l’application.

-   **La licence d’évaluation expire avant le lancement de l’application.**

    Si la version d’évaluation expire avant que l’utilisateur ne lance l’application, cette dernière ne démarre pas. Au lieu de cela, une boîte de dialogue s’affiche pour donner à l’utilisateur la possibilité d’acheter votre application dans le Windows Store.

-   **Le client achète l’application tandis que celle-ci est en cours d’exécution.**

    Si le client achète votre application tandis que celle-ci est en cours d’exécution, voici quelques actions possibles.

    -   Ne rien faire et continuer en mode d’évaluation jusqu’au redémarrage de l’application.
    -   Les remercier de leur achat ou afficher un message.
    -   Activer de manière silencieuse les fonctionnalités qui sont disponibles avec une licence complète (ou désactiver les notifications réservées à la version d’évaluation).

Si vous voulez détecter la modification de la licence et exécuter une action quelconque dans votre application, vous devez ajouter un gestionnaire d’événements, comme décrit à l’étape suivante.

## <a name="step-2-initialize-the-license-info"></a>Étape2: Initialisation des informations de licence

Lors de l’initialisation de votre application, recherchez l’objet [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) de votre application, comme indiqué dans cet exemple. Nous supposons que **licenseInformation** est une variable globale ou un champ global de type **LicenseInformation**.

Pour le moment, vous obtenez des informations de licence simulées, en utilisant [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) à la place de [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Avant de soumettre la version finale de votre application au **WindowsStore**, vous devez remplacer tous les références **CurrentAppSimulator** par des références **CurrentApp** dans votre code.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

Ensuite, ajoutez un gestionnaire d’événements pour recevoir des notifications en cas de modification de la licence lorsque l’app est en cours d’exécution. La licence de l’app peut notamment changer si la période d’évaluation arrive à expiration ou si le client achète l’app dans un Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>Étape3: Coder les fonctionnalités dans des blocs conditionnels

Lorsque l’événement de changement de licence est déclenché, votre application doit appeler l’API de licence pour déterminer si l’état de la version d’évaluation a changé. Le code de cette étape indique comment structurer votre gestionnaire pour cet événement. À ce stade, si un utilisateur a acheté l’application, il est de bonne pratique de lui indiquer que l’état de sa licence a été modifié. Demandez à l’utilisateur de redémarrer l’application si vous avez conçu votre application ainsi. Rendez cependant cette transition aussi simple que possible.

Cet exemple montre comment évaluer l’état de la licence d’une app pour activer ou désactiver une fonctionnalité particulière de votre app.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>Étape4: Obtenir la date d’expiration de la version d’évaluation d’une app

Ajoutez le code permettant de déterminer la date d’expiration de la version d’évaluation de l’application.

Le code de cet exemple définit une fonction pour obtenir la date d’expiration de la licence pour la version d’évaluation de l’application. Si cette licence est toujours valide, affichez la date d’expiration avec le nombre de jours restants avant l’échéance.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>Étape5: Test des fonctionnalités à l’aide d’appels simulés à l’API de la licence

Maintenant, testez votre application à l’aide des données simulées. **CurrentAppSimulator** obtient les informations de licence propres au test à partir d’un fichier XML appelé WindowsStoreProxy.xml, situé dans %UserProfile%\\AppData\\local\\packages\\&lt;nom_package&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Vous pouvez modifier les dates d’expiration simulées de votre application et de ses fonctionnalités dans le fichier WindowsStoreProxy.xml. Testez l’ensemble de vos configurations d’expiration et de licence possibles, afin de vous assurer que tout fonctionne comme vous le souhaitez. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).

Si ce chemin d’accès et ce fichier n’existent pas, vous devez les créer lors de l’installation ou de l’exécution. Si vous essayez d’accéder à la propriété [CurrentAppSimulator.LicenseInformation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) mais que le fichier WindowsStoreProxy.xml n’est pas présent à cet emplacement, une erreur se produit.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>Étape6: Remplacement des méthodes d’API de licence simulées par l’API réelle

Après avoir testé votre application à l’aide du serveur de licences simulées et avant d’envoyer votre application à un WindowsStore à des fins de certification, remplacez **CurrentAppSimulator** par **CurrentApp**, comme indiqué dans l’exemple de code suivant.

> [!IMPORTANT]
> Votre app doit utiliser l’objet **CurrentApp** quand vous la soumettez au Store. Dans le cas contraire, elle ne sera pas certifiée.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>Étape7: Description du fonctionnement de l'essai gratuit à vos clients

Prenez soin d’expliquer à vos clients comment votre application se comportera pendant et après la période d’évaluation gratuite afin qu’ils ne soient pas surpris par le comportement de l’application.

Pour plus d’informations sur la description de votre application, voir [Créer des descriptions d’application](https://msdn.microsoft.com/library/windows/apps/mt148529).

## <a name="related-topics"></a>Rubriques connexes

* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Définir la tarification et la disponibilité d’une application](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 
