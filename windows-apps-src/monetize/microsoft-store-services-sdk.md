---
author: mcleanbyron
Description: "Le Microsoft Store Services SDK contient des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à générer plus de revenus et conquérir des clients."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 98675e9cb06b05e55d49ca625818626aea5a5346

---

# Microsoft Store Services SDK

Le Microsoft Store Services SDK fournit des bibliothèques et des outils conçus pour vous aider à générer plus de revenus et à conquérir des clients dans vos applications de plateforme Windows universelle (UWP), comme l’affichage de publicités dans vos applications et l’exécution d’expériences avec des testsA/B. Ce kit de développement logiciel (SDK) évoluera dans le temps pour s’enrichir de nouvelles fonctionnalités conçues pour renforcer l’engagement des utilisateurs et les possibilités de monétisation.


## Fonctionnalités disponibles dans le SDK

Le Microsoft Store Services SDK fournit des bibliothèques et des outils qui prennent en charge les fonctionnalités suivantes.

### Exécuter des expériences avec des testsA/B pour les applications UWP

Exécutez des testsA/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Après avoir défini une expérience dans votre tableau de bord du Centre de développement, utilisez la classe [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) afin d’obtenir des variantes pour votre expérience dans votre application. Utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis utilisez la méthode [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) pour envoyer les événements d’affichage et de conversion au Centre de développement. Enfin, utilisez votre tableau de bord pour visualiser les résultats et pour gérer l’expérience.

Pour plus d’informations, voir [Exécuter des expériences d’application avec des testsA/B](run-app-experiments-with-a-b-testing.md).

### Commentaires sur les applications pour les applications UWP

Utilisez la classe [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) dans vos applications UWP pour diriger vos clients Windows10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Ensuite, gérez ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans le tableau de bord du Centre de développement.

Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)

### Afficher des publicités dans vos applications

Augmentez vos revenus en affichant des bannières ou des spots vidéo publicitaires de Microsoft dans les applications UWP. Vous pouvez également optimiser les taux de remplissage de vos annonces publicitaires en utilisant la médiation publicitaire pour afficher des annonces provenant de plusieurs fournisseurs de réseaux publicitaires.

Pour plus d’informations, voir [Afficher des publicités dans votre application](display-ads-in-your-app.md).

>**Remarque**&nbsp;&nbsp;Le Microsoft Store Services SDK prend uniquement en charge les applications UWP. Pour afficher des publicités dans les applicationsWindows8.1 et WindowsPhone8.x, utilisez le [Kit SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk).

### Informations de référence sur les API

Pour découvrir la documentation de référence relative aux API du Microsoft Store Services SDK, voir les [informations de référence sur les API du Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Installer le SDK

Pour installer le Microsoft Store Services SDK.

1.  Fermez toutes les instances de VisualStudio2013 ou de VisualStudio2015, puis désinstallez l’ensemble des versions précédentes du SDK d’engagement et de monétisation de la Boutique Microsoft, du Kit de développement logiciel (SDK), de l’extension AdMediator ou du Kit SDK Microsoft Advertising.
2.  Téléchargez et installez le [SDK](http://aka.ms/store-em-sdk). L’installation peut prendre quelques minutes. Attendez la fin du processus.
3.  Redémarrez VisualStudio.

Microsoft publie régulièrement de nouvelles versions du Microsoft Store Services SDK avec des améliorations des performances et de nouvelles fonctionnalités. Si certains de vos projets existants utilisent le Microsoft Store Services SDK et que vous souhaitez utiliser la dernière version de ce SDK, il vous suffit de télécharger et d’installer cette version.

Les fonctionnalités publicitaires pour les applications UWP provenant des versions précédentes du SDK d’engagement et de monétisation de la Boutique Microsoft, du Kit de développement logiciel (SDK) Universal Ad Client, de l’extension AdMediator et du Kit de SDK MicrosoftAdvertising sont désormais incluses dans le Microsoft Store Services SDK. Si vous disposez de projets UWP qui utilisent des fonctionnalités publicitaires de l’une de ces versions précédentes, vous pouvez continuer à travailler avec vos projets sans apporter aucune modification après avoir installé le Microsoft Store Services SDK.

>**Remarque** Pour installer le Microsoft Store Services SDK avec Visual Studio2015, la version1.1 ou une version ultérieure de Visual Studio Tools pour les applications Windows universelles. Pour plus d’informations sur cette mise à jour vers VisualStudioTools pour les applications Windows universelles, voir les [notes de publication](http://go.microsoft.com/fwlink/?LinkID=624516).

## Packages d’infrastructure dans le SDK

Les bibliothèques suivantes du Microsoft Store Services SDK sont configurées en tant que *packages d’infrastructure*:

* Microsoft.Advertising.dll. Cette bibliothèque contient les API publicitaires des espaces de noms [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) et [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Cette bibliothèque contient les API de l’espace de noms [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx).

Cela signifie qu’une fois que vous avez installé le SDK sur votre ordinateur de développement, ces bibliothèques sont automatiquement mises à jour par le biais de Windows Update chaque fois que nous publions de nouvelles versions des bibliothèques avec des correctifs et améliorations des performances. Ainsi, vous êtes toujours assuré de disposer de la dernière version disponible des bibliothèques installées sur votre ordinateur de développement.

En outre, une fois qu’un utilisateur a installé une version de votre application qui utilise cette bibliothèque, les bibliothèques sont également mises à jour automatiquement sur son appareil chaque fois que nous en publions de nouvelles versions avec des correctifs et des améliorations des performances. En d’autres termes, les utilisateurs disposeront toujours de la dernière version des bibliothèques, sans que vous ayez besoin de publier des versions mises à jour de votre application dans le WindowsStore.

Toutefois, si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans ces bibliothèques, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le WindowsStore.

## Rubriques connexes

* [Informations de référence sur les API du Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Exécuter des expériences avec des testsA/B](run-app-experiments-with-a-b-testing.md)
* [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)
* [Afficher des publicités dans votre application](display-ads-in-your-app.md)



<!--HONumber=Sep16_HO2-->


