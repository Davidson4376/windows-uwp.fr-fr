---
Description: The Microsoft Store Services SDK provides libraries and tools that you can use to add features to your apps that help you make more money and gain customers.
title: Impliquer les clients avec le MicrosoftStore Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 8394a1a44173541e8982a660591e84b25b985205
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7718450"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Impliquer les clients avec le MicrosoftStore Services SDK

Le Microsoft Store Services SDK fournit des fonctionnalités qui vous aident à entrer en contact avec les clients de vos applications de plateforme Windows universelle (UWP), par exemple, l’envoi de notifications ciblées à vos applications et en cours d’exécution A / B d’expériences dans vos applications. Ce kit de développement logiciel(SDK) est une extension pour VisualStudio2015 et d’autres versions de VisualStudio.

> [!NOTE]
> Pour afficher des publicités dans vos applications UWP, utilisez le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp) au lieu du MicrosoftStore Services SDK. Les bibliothèques de publicités ont été déplacées depuis le MicrosoftStore Services SDK vers le SDK MicrosoftAdvertising. Pour plus d’informations, voir [Afficher des publicités dans votre application](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Scénarios pris en charge par le MicrosoftStore Services SDK

Le MicrosoftStore Services SDK prend en charge les scénarios suivants pour les applicationsUWP. Pour découvrir la documentation de référence relative aux API, voir les [informations de référence sur les API du Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement).

|  Scénario  |  Description   |
|------------|----------------|
|  [Exécuter des expériences dans votre applicationUWP avec des testsA/B](run-app-experiments-with-a-b-testing.md)    |  Exécutez des testsA/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Après avoir défini une expérience dans l’espace partenaires, utilisez la classe [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) pour obtenir des variantes pour votre expérience dans votre application, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez et ensuite utiliser le [LogForVariation ](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)méthode pour envoyer des événements d’affichage et les événements de conversion vers l’espace partenaires. Enfin, utilisez l’espace partenaires pour afficher les résultats et gérer l’expérience.  |
|  [Lancer le Hub de commentaires à partir de votre applicationUWP](launch-feedback-hub-from-your-app.md)    |  Utilisez la classe [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) dans votre application UWP pour diriger vos clients Windows10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Ensuite, gérez ces commentaires dans le [rapport sur les commentaires](../publish/feedback-report.md) dans l’espace partenaires. |
|  [Configurer votre application UWP pour recevoir des notifications push l’espace partenaires](configure-your-app-to-receive-dev-center-notifications.md)    |  Utilisez la classe [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) dans votre application UWP pour inscrire votre application pour recevoir des notifications push ciblées que vous envoyez à vos clients à l’aide de l’espace partenaires.  |
|   [Consigner des événements personnalisés dans votre application UWP pour le rapport d’utilisation dans l’espace partenaires](log-custom-events-for-dev-center.md)   |  Utilisez la classe [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) dans votre application UWP pour consigner des événements personnalisés qui sont associés à votre application dans l’espace partenaires. Ensuite, passez en revue le total d’occurrences de vos événements personnalisés dans la section **événements personnalisés** du [rapport d’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) dans l’espace partenaires.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Conditions préalables

Le Microsoft Store Services SDK nécessite les éléments suivants:

* Visual Studio 2015 ou une version ultérieure.
* VisualStudioTools pour applicationsWindowsuniverselles, installé avec votre version de VisualStudio.

<span id="install" />

## <a name="install-the-sdk"></a>Installer le SDK

Il existe deux options pour installer le Microsoft Store Services SDK sur votre ordinateur de développement:

* **Programme d’installation MSI**&nbsp;&nbsp;Vous pouvez installer le SDK via le programme d’installation MSI, disponible [ici](http://aka.ms/store-em-sdk).
* **Package NuGet**&nbsp;&nbsp;Vous pouvez installer le Kit de développement logiciel sous forme de package NuGet.

Microsoft publie régulièrement de nouvelles versions du Microsoft Store Services SDK avec des améliorations des performances et de nouvelles fonctionnalités. Si vous possédez des projets existants qui utilisent le SDK et que vous souhaitez utiliser la dernière version, téléchargez et installez la dernière version du SDK sur votre ordinateur de développement.

<span id="install-msi" />

### <a name="install-via-msi"></a>Installer via MSI

Pour installer le Microsoft Store Services SDK via le programme d’installationMSI:

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé le SDK Microsoft Store Engagement and Monetization, le SDK UniversalAdClient ou l’extensionAdMediator, désinstallez ces SDK maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Téléchargez et installez le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Redémarrez VisualStudio.

5.  Si vous possédez un projet existant qui référence des bibliothèques d’une version antérieure du Microsoft Store Services SDK, du SDK MicrosoftAdvertising, du SDKUniversal Ad Client ou du SDK Microsoft Store Engagement and Monetization, nous vous recommandons d’ouvrir votre projet dans VisualStudio, puis de le nettoyer et le régénérer (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet et sélectionnez **Nettoyer**. Ensuite, cliquez de nouveau avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence assembly à votre projet](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Installer via NuGet

Pour installer les bibliothèques du Microsoft Store Services SDK via NuGet:

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé le SDK Microsoft Store Engagement and Monetization, le SDK UniversalAdClient ou l’extensionAdMediator, désinstallez ces SDK maintenant. Si vous le souhaitez, ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Démarrez VisualStudio, puis ouvrez le projet dans lequel vous souhaitez utiliser le Microsoft Store Services SDK.
    > [!NOTE]
    > Si votre projet inclut déjà des références de bibliothèque d’une installation MSI antérieure du SDK, supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans VisualStudio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

5. Dans la zone de recherche, entrez **Microsoft.Services.Store.Engagement**, puis installez le package Microsoft.Services.Store.Engagement. Lorsque l’installation du package est terminée, enregistrez votre solution.
    > [!NOTE]
    > Si la fenêtre **Sortie** signale une erreur *Install-Package* qui fait état d’une longueur trop importante du chemin spécifié, il vous faudra éventuellement configurer NuGet pour l’extraction des packages vers un autre emplacement présentant un chemin plus court que l’emplacement par défaut. Pour ce faire, ajoutez la valeur ```repositoryPath``` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projetVisual Studio vers un dossier différent présentant un chemin plus court.

6. Fermez la solution Visual Studio qui contient votre projet, puis rouvrez la solution.

7.  Si votre projet référence déjà des bibliothèques d’une version antérieure du Microsoft Store Services SDK installée via NuGet et que vous avez mis à jour votre projet vers une version plus récente du SDK, nous vous recommandons de nettoyer et de régénérer votre projet (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Nettoyer**, cliquez de nouveau avec le bouton droit sur le nœud de projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter une référence assembly à votre projet](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Ajoutez la référence assembly à votre projet

Une fois que vous avez installé le Microsoft Store Services SDK via le programme d’installationMSI ou NuGet, suivez ces instructions pour référencer le kit SDK assembly dans votre projetUWP.

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si votre projet est une applicationJavaScript qui cible **Toute CU**, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple,  **x86**).

2. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

3. Dans le **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis sélectionnez la case à cocher en regard de **Microsoft Engagement Framework**. Cela vous permet d'utiliser les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

3. Cliquez sur**OK**.

> [!NOTE]
> Si vous avez installé les bibliothèques SDK via NuGet, votre projet comportera une référence **Microsoft.Services.Store.Engagement**. La référence **Microsoft.Services.Store.Engagement** représente le package NuGet (non pas les bibliothèques qu’il contient); vous pouvez l’ignorer.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Présentation des packages d’infrastructure dans le SDK

La bibliothèque Microsoft.Services.Store.Engagement.dll du kit MicrosoftStore Services SDK est configurée comme un *package d’infrastructure*. Cette bibliothèque contient les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

Cette bibliothèque étant un package d’infrastructure, cela signifie que dès qu’un utilisateur installe une version de votre application qui l’utilise, cette bibliothèque sera automatiquement mise à jour sur l’appareil via WindowsUpdate dès la publication d’une nouvelle version de la bibliothèque intégrant des correctifs et améliorant ses performances. Ainsi, vos clients sont toujours assurés de disposer de la dernière version disponible de la bibliothèque sur leurs appareils.

Si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le WindowsStore.

## <a name="related-topics"></a>Rubriques connexes

* [Informations de référence sur les API du Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement)
* [Exécuter des expériences avec des testsA/B](run-app-experiments-with-a-b-testing.md)
* [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)
* [Configurer votre application pour recevoir des notifications push l’espace partenaires](configure-your-app-to-receive-dev-center-notifications.md)
* [Consigner des événements personnalisés pour l’Espace partenaires](log-custom-events-for-dev-center.md)
