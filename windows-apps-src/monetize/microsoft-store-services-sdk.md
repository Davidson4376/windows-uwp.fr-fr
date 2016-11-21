---
author: mcleanbyron
Description: "Le Microsoft Store Services SDK contient des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à générer plus de revenus et conquérir des clients."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 011b370b7bd7ad7c7d8f60281261b6da954e2256
ms.openlocfilehash: 840a5e76d409f547d55e558262af09c8fa36a544

---

# Microsoft Store Services SDK

Le Microsoft Store Services SDK fournit des fonctionnalités qui vous permettent de générer davantage de revenus et de conquérir des clients dans vos applicationsUWP, comme l’affichage de publicités et l’exécution d’expériences avec des tests A/B. Ce kit de développement logiciel(SDK) est une extension pour VisualStudio2015 et d’autres versions de VisualStudio.

## Scénarios pris en charge par le SDK

Ce kit de développement logiciel prend en charge les scénarios suivants pour les applicationsUWP. Il évoluera dans le temps pour renforcer l’engagement des utilisateurs et les possibilités de monétisation. Pour découvrir la documentation de référence relative aux API du SDK consultez les [informations de référence sur les API du Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

|  Scénario  |  Description   |
|------------|----------------|
|  [Exécuter des expériences dans votre applicationUWP avec des testsA/B](run-app-experiments-with-a-b-testing.md)    |  Exécutez des testsA/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Après avoir défini une expérience dans votre tableau de bord du Centre de développement, utilisez la classe [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) afin d’obtenir des variantes pour votre expérience dans votre application. Utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis utilisez la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) pour envoyer les événements d’affichage et de conversion au Centre de développement. Enfin, utilisez votre tableau de bord pour visualiser les résultats et pour gérer l’expérience.  |
|  [Lancer le Hub de commentaires à partir de votre applicationUWP](launch-feedback-hub-from-your-app.md)    |  Utilisez la classe [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) dans votre application UWP pour diriger vos clients Windows10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Ensuite, gérez ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans le tableau de bord du Centre de développement. |
|  [Configurer votre applicationUWP pour recevoir des notifications Push du Centre de développement](configure-your-app-to-receive-dev-center-notifications.md)    |  Utilisez la classe [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) dans votre applicationUWP afin de l’inscrire pour la réception de notificationsPush ciblées que vous envoyez à vos clients à l’aide du tableau de bord du Centre de développement Windows.  |
|   [Consigner des événements personnalisés dans votre applicationUWP, pour le rapport d’utilisation du Centre de développement](log-custom-events-for-dev-center.md)   |  Utilisez la classe [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) de votre applicationUWP pour consigner des événements personnalisés qui sont associés à votre application dans le Centre de développement. Ensuite, passez en revue le total d’occurrences de vos événements personnalisés dans la section **Événements personnalisés** du [Rapport sur l’utilisation](https://msdn.microsoft.com/windows/uwp/publish/usage-report) du tableau de bord du Centre de développement.  |
|  [Afficher des publicités dans votre applicationUWP](display-ads-in-your-app.md)    |  Utilisez les contrôles [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) de votre applicationUWP pour augmenter vos revenus en affichant les bannières publicitaires ou les spots vidéo publicitaires.<br/><br/>**Remarque**&nbsp;&nbsp;Le Microsoft Store Services SDK prend uniquement en charge les applicationsUWP pour Windows10. Pour afficher des publicités dans les applicationsWindows8.1 et WindowsPhone8.x, utilisez le [Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk).  |

<span id="prerequisites" />
## Prérequis

Le Microsoft Store Services SDK nécessite les éléments suivants:

* Visual Studio 2015 ou une version ultérieure.
* VisualStudioTools pour applicationsWindowsuniverselles installé avec votre version de VisualStudio.

>**Remarque**&nbsp;&nbsp;Pour installer le SDK avec VisualStudio2015, vous devez avoir installé la version1.1 ou une version ultérieure de Visual Studio Tools pour applications Windows universelles. Pour plus d’informations sur cette mise à jour vers VisualStudioTools pour les applications Windows universelles, voir les [notes de publication](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install" />
## Installer le SDK

Il existe deuxoptions d’installation du Microsoft Store Services SDK pour une utilisation avec VisualStudio2015 (ou une version ultérieure) sur votre ordinateur de développement:

* **Programme d’installation MSI**&nbsp;&nbsp;Vous pouvez installer le SDK via le programme d’installation MSI, disponible [ici](http://aka.ms/store-em-sdk). Avec cette option, les bibliothèquesdu SDK sont installées dans un emplacement partagé sur votre ordinateur de développement. Ainsi, elles peuvent être référencées par tout projetUWP dans VisualStudio.
* **PackageNuGet**&nbsp;&nbsp;Vous pouvez installer les bibliohtèquesdu SDK pour un projetUWP spécifique dans VisualStudio, à l’aide de NuGet. Avec cette option, les bibliothèques du SDK sont installées uniquement pour le projet dans lequel vous avez installé le package NuGet.

Microsoft publie régulièrement de nouvelles versions du Microsoft Store Services SDK avec des améliorations des performances et de nouvelles fonctionnalités. Si vous possédez des projets existants qui utilisent le SDK et que vous souhaitez utiliser la dernière version, téléchargez et installez la dernière version du SDK sur votre ordinateur de développement.

>**Remarque**&nbsp;&nbsp;Pour installer le SDK avec VisualStudio2015, vous devez avoir installé la version1.1 ou une version ultérieure de Visual Studio Tools pour applications Windows universelles. Pour plus d’informations sur cette mise à jour vers VisualStudioTools pour les applications Windows universelles, voir les [notes de publication](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install-msi" />
### Installer via MSI

Pour installer le Microsoft Store Services SDK via le programme d’installationMSI:

1.  Fermez toutes les instances de VisualStudio2015 (ou version ultérieure). Si vous aviez précédemment installé une version antérieure du Kit de développement MicrosoftAdvertising, du kit MicrosoftUniversalAdClient, de l’extensionAdMediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant.

2.  Ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Téléchargez et installez le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Redémarrez VisualStudio.

5.  Si vous possédez un projet existant qui référence des bibliothèques d’une version antérieure du Microsoft Store Services SDK, du Kit de développement MicrosoftAdvertising, du KitUniversal Ad Client ou du SDK d’engagement et de monétisation de la Boutique Microsoft, nous vous recommandons d’ouvrir votre projet dans VisualStudio, puis de le nettoyer et le régénérer (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet et sélectionnez **Nettoyer**. Ensuite, cliquez de nouveau avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter les références de bibliothèques appropriées du Microsoft Store Services SDK dans votre projet](#references).

<span id="install-nuget" />
### Installer via NuGet

Pour installer les bibliothèquesMicrosoft Store Services SDK pour un projet spécifique via NuGet:

1.  Fermez toutes les instances de VisualStudio2015 (ou version ultérieure). Si vous aviez précédemment installé une version antérieure du Kit de développement MicrosoftAdvertising, du kit MicrosoftUniversalAdClient, de l’extensionAdMediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant.

2.  Ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Démarrez VisualStudio, puis ouvrez le projet dans lequel vous souhaitez utiliser les bibliothèquesdu Microsoft Store Services SDK.

  >**Remarque**&nbsp;&nbsp;Si votre projet inclut déjà des références de bibliothèque d’une installation antérieure du SDK, supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans VisualStudio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

5. Dans la zone de recherche, entrez **Microsoft.Services.Store.SDK**, puis installez le packageMicrosoft.Services.Store.SDK.

  >**Remarque**&nbsp;&nbsp;Si la fenêtre **Sortie** signale une erreur *Install-Package* qui fait état d’une longueur trop importante du chemin spécifié, il vous faudra éventuellement configurer NuGet pour l’extraction des packages vers un autre emplacement présentant un chemin plus court que l’emplacement par défaut. Pour ce faire, ajoutez la valeur ```repositoryPath``` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projetVisual Studio vers un dossier différent présentant un chemin plus court.

6. Fermez votre projet, puis rouvrez-le.

7.  Si votre projet référence déjà des bibliothèques d’une version antérieure du Microsoft Store Services SDK installée via NuGet et que vous avez mis à jour votre projet vers une version plus récente du SDK, nous vous recommandons de nettoyer et de régénérer votre projet (dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Nettoyer**, cliquez de nouveau avec le bouton droit sur le nœud de projet, puis sélectionnez **Régénérer**).

  Sinon, si vous utilisez le SDK pour la première fois dans votre projet, vous pouvez désormais [ajouter les références de bibliothèques appropriées du Microsoft Store Services SDK dans votre projet](#references).

<span id="references" />
## Ajouter des références de bibliothèques du SDK à votre projet

Une fois que vous avez installé le Microsoft Store Services SDK via le programme d’installationMSI ou NuGet, suivez ces instructions pour référencer les bibliothèques du SDK dans votre projetUWP.

1. Ouvrez votre projet dans VisualStudio.

  >**Remarque**&nbsp;&nbsp;Si votre projet est une applicationJavascript qui cible **Toute UC**, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**).

2. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

3. Dans le **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de l’un des éléments suivants.

  * Pour utiliser les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx)pour les scénarios d’engagement client, sélectionnez la case en regard de **Microsoft Engagement Framework**. Sélectionnez cette option si vous souhaitez [exécuter des testsA/B](run-app-experiments-with-a-b-testing.md), [lancer le hub de commentaires](launch-feedback-hub-from-your-app.md), [recevoir des notificationsPush ciblées du Centre de développement](configure-your-app-to-receive-dev-center-notifications.md) ou [consigner des événements personnalisés sur le Centre de développement](log-custom-events-for-dev-center.md).

  * Pour utiliser les API pour [afficher des bannières publicitaires ou des spots vidéos publicitaires dans votre application](display-ads-in-your-app.md), sélectionnez la case à cocher en regard de **Kit de développement logiciel (SDK) MicrosoftAdvertising pour XAML** ou de **Kit de développement logiciel (SDK) MicrosoftAdvertising pour JavaScript**, en fonction de votre type de projet.

3. Cliquez sur **OK**.

>**Remarque**&nbsp;&nbsp;SI vous avez installé les bibliothèques du SDK via NuGet,votre projet comportera une référence **Microsoft.Services.Store.SDK** en complément de **Kit de développement logiciel (SDK) MicrosoftAdvertising pour XAML** ou de **Kit de développement logiciel (SDK) MicrosoftAdvertising pour JavaScript**. La référence **Microsoft.Services.Store.SDK** représente le package NuGet (non pas les bibliothèques qu’il contient); vous pouvez l’ignorer.

<span id="framework" />
## Présentation des packages d’infrastructure dans le SDK

Les bibliothèques suivantes du Microsoft Store Services SDK sont configurées en tant que *packages d’infrastructure*:

* Microsoft.Advertising.dll. Cette bibliothèque contient les API publicitaires des espaces de noms [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) et [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Cette bibliothèque contient les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx).

Cela signifie qu’une fois qu’un utilisateur installe une version de votre application qui utilise ces bibliothèques, ces dernières sont automatiquement mises à jour sur leur appareil via WindowsUpdate lors de la publication de nouvelles versions des bibliothèques avec des correctifs et des améliorations de performance. Ainsi, vos clients sont toujours assurés de disposer de la dernière version disponible de ces bibliothèques sur leurs appareils.

Si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans ces bibliothèques, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le WindowsStore.

## Rubriques connexes

* [Informations de référence sur les API du Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Exécuter des expériences avec des testsA/B](run-app-experiments-with-a-b-testing.md)
* [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)
* [Configurer votre application pour recevoir des notifications push du Centre de développement](configure-your-app-to-receive-dev-center-notifications.md)
* [Consigner des événements personnalisés pour le Centre de développement](log-custom-events-for-dev-center.md)
* [Afficher des publicités dans votre application](display-ads-in-your-app.md)



<!--HONumber=Nov16_HO1-->


