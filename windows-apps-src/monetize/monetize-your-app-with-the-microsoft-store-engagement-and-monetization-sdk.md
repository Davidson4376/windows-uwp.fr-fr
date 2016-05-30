---
author: mcleanbyron
Description: Le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft fournit des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à générer plus de revenus et à conquérir des clients.
title: SDK d’engagement et de monétisation de la Boutique Microsoft
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
---

# SDK d’engagement et de monétisation de la Boutique Microsoft

Le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft fournit des bibliothèques et des outils conçus pour vous aider à générer plus de revenus et à conquérir des clients, comme l’affichage de publicités dans vos applications et l’exécution d’expériences avec des tests A/B. Ce SDK remplace le SDK Microsoft Universal Ad Client, et évoluera au fil du temps afin d’inclure de nouvelles fonctionnalités d’engagement et de monétisation.


## Fonctionnalités disponibles dans le SDK

Le SDK d’engagement et de monétisation de la Boutique Microsoft fournit des bibliothèques et des outils qui prennent en charge les fonctionnalités suivantes.

### Exécuter des expériences avec des tests A/B pour les applications UWP

Exécutez des tests A/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Après avoir défini une expérience dans votre tableau de bord du Centre de développement, utilisez la classe [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) afin d’obtenir des variantes pour votre expérience dans votre application. Utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis utilisez la méthode [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) pour envoyer les événements d’affichage et de conversion au Centre de développement. Enfin, utilisez votre tableau de bord pour visualiser les résultats et pour gérer l’expérience.

Pour plus d’informations, voir [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md).

### Commentaires sur les applications pour les applications UWP

Utilisez la classe [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) dans vos applications UWP pour diriger vos clients Windows 10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Ensuite, gérez ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans le tableau de bord du Centre de développement.

Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)

>**Remarque** Pour le moment, le **Rapport sur les commentaires** est disponible uniquement pour les comptes de développeur qui ont rejoint le [Programme Insider du Centre de développement](../publish/dev-center-insider-program.md).

### Afficher des publicités dans vos applications

Augmentez vos revenus en affichant des bannières publicitaires ou des spots vidéo publicitaires de Microsoft dans les applications UWP, ainsi que dans les applications Windows 8.1 et Windows Phone 8.x. Vous pouvez également optimiser les taux de remplissage de vos annonces publicitaires en utilisant la médiation publicitaire pour afficher des annonces provenant de plusieurs fournisseurs de réseau publicitaire.

Pour plus d’informations, voir [Afficher des publicités dans votre application](display-ads-in-your-app.md).

>**Remarque** Les fonctionnalités publicitaires issues des versions précédentes du SDK Universal Ad Client, de l’extension Ad Mediator et du SDK Microsoft Advertising sont désormais incluses dans le SDK d’engagement et de monétisation de la Boutique Microsoft.

### Informations de référence sur les API

Pour découvrir la documentation de référence relative aux API du SDK, voir [Informations de référence sur les API du Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Installer le SDK

Pour installer le SDK d’engagement et de monétisation de la Boutique Microsoft :

1.  Fermez toutes les instances de Visual Studio 2013 ou de Visual Studio 2015, puis désinstallez l’ensemble des versions précédentes du SDK Universal Ad Client, de l’extension Ad Mediator ou du SDK Microsoft Advertising.
2.  Téléchargez et installez le [SDK](http://aka.ms/store-em-sdk). L’installation peut prendre quelques minutes. Attendez la fin du processus.
3.  Redémarrez Visual Studio.

Microsoft publie régulièrement de nouvelles versions du SDK d’engagement et de monétisation de la Boutique Microsoft avec des améliorations des performances et de nouvelles fonctionnalités. Si certains de vos projets existants utilisent le SDK d’engagement et de monétisation de la Boutique Microsoft et que vous souhaitez utiliser la dernière version de ce SDK, il vous suffit de télécharger et d’installer cette version.

Les fonctionnalités publicitaires provenant des versions précédentes du SDK Universal Ad Client, de l’extension Ad Mediator et du SDK Microsoft Advertising sont désormais incluses dans le SDK d’engagement et de monétisation de la Boutique Microsoft. Si vous disposez de projets Visual Studio 2015 ou Visual Studio 2013 qui utilisent des fonctionnalités publicitaires de l’une de ces versions précédentes, vous pouvez continuer à travailler avec vos projets sans apporter aucune modification après avoir installé le SDK d’engagement et de monétisation de la Boutique Microsoft.

>**Remarque** Pour installer le SDK d’engagement et de monétisation de la Boutique Microsoft avec Visual Studio 2015, vous devez disposer de la version 1.1 ou ultérieure de Visual Studio Tools pour les applications Windows universelles. Pour plus d’informations sur cette mise à jour de Visual Studio Tools pour les applications Windows universelles, consultez les [notes de publication](http://go.microsoft.com/fwlink/?LinkID=624516).

## Packages d’infrastructure dans le SDK

La bibliothèque ci-après du SDK d’engagement et de monétisation de la Boutique Microsoft est configurée sous la forme d’un *package d’infrastructure* :

* Microsoft.Advertising.dll (pour les projets d’application UWP uniquement). Cette bibliothèque contient les API publicitaires des espaces de noms **Microsoft.Advertising** et **Microsoft.Advertising.WinRT.UI**.

Cela signifie qu’une fois que vous avez installé le SDK sur votre ordinateur de développement, cette bibliothèque est automatiquement mise à jour par le biais de Windows Update chaque fois que nous publions de nouvelles versions des bibliothèques avec des correctifs et améliorations des performances. Ainsi, vous êtes toujours assuré de disposer de la dernière version disponible de la bibliothèque installée sur votre ordinateur de développement.

En outre, une fois qu’un utilisateur a installé une version de votre application qui utilise cette bibliothèque, la bibliothèque est également mise à jour automatiquement sur l’appareil de l’utilisateur chaque fois que nous en publions de nouvelles versions avec des correctifs et des améliorations des performances. En d’autres termes, les utilisateurs disposeront toujours de la dernière version de la bibliothèque, sans que vous ayez besoin de publier des versions mises à jour de votre application dans le Windows Store.

Toutefois, si nous publions une nouvelle version du SDK qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du SDK pour bénéficier de ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le Windows Store.

Les autres bibliothèques du SDK, notamment Microsoft.Advertising.dll pour d’autres plateformes cibles et les bibliothèques relatives à la médiation publicitaire, ne sont pas configurées comme bibliothèques d’infrastructure pour l’instant.

## Rubriques connexes

* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
* [Informations de référence sur les API du Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)


<!--HONumber=May16_HO2-->


