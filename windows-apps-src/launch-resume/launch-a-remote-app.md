---
author: TylerMSFT
title: Lancer une application sur un appareil distant
description: "Découvrez comment lancer une application sur un appareil distant à l’aide du projet «Rome»."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# Lancer une application sur un appareil distant

Cet article explique comment lancer une application de plateforme Windows universelle (UWP) ou une application de bureau Windows sur un appareil distant.

Depuis Windows10 version1607, une applicationUWP peut lancer une applicationUWP ou une application de bureau Windows à distance sur un autre appareil qui exécute également Windows10 version1607 ou ultérieure.

Le lancement d’une application sur un appareil distant permet notamment à un utilisateur de démarrer une tâche sur un appareil et de la terminer sur un autre. Par exemple, si vous écoutez de la musique sur votre téléphone dans votre voiture sur le chemin du retour à la maison, vous pourriez utiliser le lancement à distance pour basculer la lecture sur votre Xbox chez vous. Vous pouvez transmettre des données à l’application distante pour lui indiquer le contexte dans lequel poursuivre la tâche.

## Ajoutez la fonctionnalité Système distant

Pour que votre application puisse lancer une application sur un appareil distant, vous devez ajouter la fonctionnalité &lt;uap3:Capability Name="remoteSystem"/&gt; dans le manifeste du package de votre application. Vous pouvez l’ajouter à l’aide du concepteur de manifeste de package en sélectionnant **Système distant** dans l’onglet **Capacités**, ou manuellement, comme le concepteur de manifeste de package, en ajoutant ce qui suit dans votre fichier Package.appxmanifest.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## Trouver un appareil distant

Vous devez tout d’abord trouver l’appareil auquel vous souhaitez vous connecter. La section [Détecter les périphériques distants](discover-remote-devices.md) explique en détail comment faire. Nous allons utiliser une approche simple, qui permet un filtrage par type d’appareil ou de connectivité. Nous allons créer un observateur qui recherche des appareils distants et écrire des gestionnaires d’événements qui sont avertis lorsque des appareils utilisant le même compte Microsoft sont détectés ou supprimés. Cela nous fournira un ensemble d’appareils distants.

Le code de ces exemples suppose que votre fichier contient une instruction `using Windows.System.RemoteSystems`.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

La première chose à faire avant d’effectuer lancement à distance est d’appeler `RemoteSystem.RequestAccessAsync()`. Vérifiez la valeur de retour pour vous assurer que votre application est autorisée à accéder à des appareils distants. Cette vérification peut échouer si vous n’avez ajouté la fonctionnalité `remoteSystem` à votre application.

Les gestionnaires d’événements de l’Observateur du système sont appelés lorsqu’un appareil auquel nous pouvons nous connecter est détecté ou n’est plus disponible. Nous allons utiliser ces gestionnaires d’événements pour conserver une liste mise à jour d’appareils auxquels nous pouvons nous connecter.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

Nous avons assuré le suivi des appareils par ID de système distant, à l’aide d’un dictionnaire. Un objet ObservableCollection est utilisé pour contenir la liste des appareils que nous pouvons énumérer. Un objet ObservableCollection facilite aussi la liaison de la liste des appareils à l’interface utilisateur, mais nous nous en dispenserons dans cet exemple.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Ajoutez un appel à `BuildDeviceList()` dans le code de démarrage de l’application avant d’essayer de lancer une application à distance.

## Lancer une application sur un appareil distant

Lancez une application à distance en transmettant l’appareil auquel vous souhaitez vous connecter à l’API [RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx). Il existe troissurcharges pour cette fonction. La plus simple, qui illustre cet exemple, spécifie l’URI qui active l’application sur l’appareil distant. Dans cet exemple, l’URI ouvre l’application Cartes sur l’ordinateur distant avec une vue3D de l’Aiguille de l'espace de Seattle.

Les autres surcharges de **RemoteLauncher.LaunchUriAsync** vous permettent de spécifier des options telles que l’URI du site web à afficher si une application capable de gérer l’URI ne peut pas être lancée sur l’appareil distant et une liste facultative de noms de famille de package pouvant servir à lancer l’URI sur l’appareil distant. Vous pouvez également fournir des données au format de paires clé/valeur. Vous pouvez transmettre des données à l’application que vous activez sur l’appareil distant pour fournir un contexte à l’application distante, comme le nom de la chanson à lire et l’emplacement de la lecture en cours lorsque vous basculez la lecture d’un appareil à un autre.

Concrètement, vous pouvez fournir l’interface utilisateur pour sélectionner l’appareil que vous souhaitez utiliser. Mais pour simplifier cet exemple, nous allons simplement utiliser le premier point de la liste pour effectuer l’appel à distance.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

L’objet [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx) renvoyé par **RemoteLauncher.LaunchUriAsync()** indique sur le lancement à distance a abouti ou, dans le cas contraire, la raison de l’échec.

## Rubriques connexes

[Référence sur l’API Systèmes distants](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[Applications et appareils connectés (projet «Rome»)](connected-apps-and-devices.md)  
L’[exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) montre comment détecter un système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages avec des applications qui s’exécutent sur deuxsystèmes.



<!--HONumber=Aug16_HO5-->


