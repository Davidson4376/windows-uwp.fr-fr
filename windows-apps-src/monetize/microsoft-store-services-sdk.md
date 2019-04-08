---
Description: Le Microsoft Store Services SDK contient des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à générer plus de revenus et conquérir des clients.
title: Impliquer les clients avec le Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 24ec2013735597efae73aee31bb4aee1a8e1413e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594984"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Impliquer les clients avec le Microsoft Store Services SDK

Le de Microsoft Store Services SDK fournit des fonctionnalités qui vous aident à collaborer avec les clients dans vos applications Universal Windows Platform (UWP), comme l’envoi des notifications ciblées à vos applications et en cours d’exécution A / B expériences dans vos applications. Ce kit de développement logiciel (SDK) est une extension pour Visual Studio 2015 et d’autres versions de Visual Studio.

> [!NOTE]
> Pour afficher des publicités dans vos applications UWP, utilisez le [SDK Microsoft Advertising](https://aka.ms/ads-sdk-uwp) au lieu du Microsoft Store Services SDK. Les bibliothèques de publicités ont été déplacées depuis le Microsoft Store Services SDK vers le SDK Microsoft Advertising. Pour plus d’informations, voir [Afficher des publicités dans votre application](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Scénarios pris en charge par le Microsoft Store Services SDK

Le Microsoft Store Services SDK prend en charge les scénarios suivants pour les applications UWP. Pour découvrir la documentation de référence relative aux API, voir les [informations de référence sur les API du Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement).

|  Scénario  |  Description   |
|------------|----------------|
|  [Exécuter des expériences dans votre application UWP avec un test a / B](run-app-experiments-with-a-b-testing.md)    |  Exécutez des tests A/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Après avoir défini une expérience dans l’espace partenaires, utilisez le [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) classe pour obtenir des variations pour votre expérience dans votre application, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis utiliser la [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) méthode pour envoyer des événements d’affichage et les événements de conversions à Partner Center. Enfin, utilisez des partenaires pour afficher les résultats et de gérer l’expérience.  |
|  [Lancer le Hub de commentaires à partir de votre application UWP](launch-feedback-hub-from-your-app.md)    |  Utilisez la classe [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) dans votre application UWP pour diriger vos clients Windows 10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Gérez ensuite ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans l’Espace partenaires. |
|  [Configurer votre application UWP pour recevoir des notifications push de partenaires](configure-your-app-to-receive-dev-center-notifications.md)    |  Utilisez le [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) classe dans votre application UWP pour inscrire votre application pour recevoir des notifications push ciblées que vous envoyez à vos clients à l’aide de partenaires.  |
|   [Journal des événements personnalisés dans votre application UWP pour le rapport d’utilisation de partenaires](log-custom-events-for-dev-center.md)   |  Utilisez le [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) classe dans votre application UWP pour enregistrer des événements personnalisés qui sont associés à votre application dans le centre de partenaires. Ensuite, passez en revue le nombre total d’occurrences pour vos événements personnalisés dans le **événements personnalisés** section de la [rapport d’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) dans Partner Center.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Conditions préalables

Le Microsoft Store Services SDK nécessite les éléments suivants :

* Visual Studio 2015 ou une version ultérieure.
* Visual Studio Tools pour applications Windows universelles installé avec votre version de Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Installer le SDK

Il existe deux options pour installer le Microsoft Store Services SDK sur votre ordinateur de développement :

* **Programme d’installation MSI**&nbsp;&nbsp;Vous pouvez installer le SDK via le programme d’installation MSI, disponible [ici](https://aka.ms/store-em-sdk).
* **Package NuGet**&nbsp;&nbsp;Vous pouvez installer le Kit de développement logiciel sous forme de package NuGet.

Microsoft publie régulièrement de nouvelles versions du Microsoft Store Services SDK avec des améliorations des performances et de nouvelles fonctionnalités. Si vous possédez des projets existants qui utilisent le SDK et que vous souhaitez utiliser la dernière version, téléchargez et installez la dernière version du SDK sur votre ordinateur de développement.

<span id="install-msi" />

### <a name="install-via-msi"></a>Installer via MSI

Pour installer le Microsoft Store Services SDK via le programme d’installation MSI :

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé le SDK Microsoft Store Engagement and Monetization, le SDK Universal Ad Client ou l’extension Ad Mediator, désinstallez ces SDK maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur :
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Téléchargez et installez le [Microsoft Store Services SDK](https://aka.ms/store-em-sdk). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Redémarrez Visual Studio.

5.  Si vous possédez un projet existant qui référence des bibliothèques d’une version antérieure du Microsoft Store Services SDK, du SDK Microsoft Advertising, du SDK Universal Ad Client ou du SDK Microsoft Store Engagement and Monetization, nous vous recommandons d’ouvrir votre projet dans Visual Studio, puis de le nettoyer et le régénérer (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet et sélectionnez **Nettoyer**. Ensuite, cliquez de nouveau avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence assembly à votre projet](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Installer via NuGet

Pour installer les bibliothèques du Microsoft Store Services SDK via NuGet :

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé le SDK Microsoft Store Engagement and Monetization, le SDK Universal Ad Client ou l’extension Ad Mediator, désinstallez ces SDK maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur :
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Démarrez Visual Studio, puis ouvrez le projet dans lequel vous souhaitez utiliser le Microsoft Store Services SDK.
    > [!NOTE]
    > Si votre projet inclut déjà des références de bibliothèque d’une installation MSI antérieure du SDK, supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans Visual Studio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

5. Dans la zone de recherche, entrez **Microsoft.Services.Store.Engagement**, puis installez le package Microsoft.Services.Store.Engagement. Lorsque l’installation du package est terminée, enregistrez votre solution.
    > [!NOTE]
    > Si la fenêtre **Sortie** signale une erreur *Install-Package* qui fait état d’une longueur trop importante du chemin spécifié, il vous faudra éventuellement configurer NuGet pour l’extraction des packages vers un autre emplacement présentant un chemin plus court que l’emplacement par défaut. Pour ce faire, ajoutez la valeur ```repositoryPath``` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](https://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projet Visual Studio vers un dossier différent présentant un chemin plus court. Le problème peut également être dû à votre chemin d’accès de packages globaux est trop long. Dans ce cas, ajoutez la ```globalPackagesFolder``` valeur dans votre fichier nuget.config.

6. Fermez la solution Visual Studio qui contient votre projet, puis rouvrez la solution.

7.  Si votre projet référence déjà des bibliothèques d’une version antérieure du Microsoft Store Services SDK installée via NuGet et que vous avez mis à jour votre projet vers une version plus récente du SDK, nous vous recommandons de nettoyer et de régénérer votre projet (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Nettoyer**, cliquez de nouveau avec le bouton droit sur le nœud de projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence assembly à votre projet](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Ajoutez la référence assembly à votre projet

Une fois que vous avez installé le Microsoft Store Services SDK via le programme d’installation MSI ou NuGet, suivez ces instructions pour référencer le kit SDK assembly dans votre projet UWP.

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si votre projet est une application JavaScript qui cible **Toute CU**, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple,  **x86**).

2. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

3. Dans le **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis sélectionnez la case à cocher en regard de **Microsoft Engagement Framework**. Cela vous permet d'utiliser les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

3. Cliquez sur **OK**.

> [!NOTE]
> Si vous avez installé les bibliothèques SDK via NuGet, votre projet comportera une référence **Microsoft.Services.Store.Engagement**. La référence **Microsoft.Services.Store.Engagement** représente le package NuGet (non pas les bibliothèques qu’il contient) ; vous pouvez l’ignorer.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Présentation des packages d’infrastructure dans le SDK

La bibliothèque Microsoft.Services.Store.Engagement.dll du kit Microsoft Store Services SDK est configurée comme un *package d’infrastructure*. Cette bibliothèque contient les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

Cette bibliothèque étant un package d’infrastructure, cela signifie que dès qu’un utilisateur installe une version de votre application qui l’utilise, cette bibliothèque sera automatiquement mise à jour sur l’appareil via Windows Update dès la publication d’une nouvelle version de la bibliothèque intégrant des correctifs et améliorant ses performances. Ainsi, vos clients sont toujours assurés de disposer de la dernière version disponible de la bibliothèque sur leurs appareils.

Si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le Windows Store.

## <a name="related-topics"></a>Rubriques connexes

* [Référence de l’API de Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement)
* [Faire des expériences avec les tests A/B](run-app-experiments-with-a-b-testing.md)
* [Démarrer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)
* [Configurer votre application pour recevoir des notifications Push de l’Espace partenaires](configure-your-app-to-receive-dev-center-notifications.md)
* [Journaliser des événements personnalisés pour l’Espace partenaires](log-custom-events-for-dev-center.md)
